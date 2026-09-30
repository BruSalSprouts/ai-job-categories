# Final Report: Understanding AI Job Categories and Predicting Salary Midpoints

*Author: Bruno | Springboard Data Science Capstone*

*Draft status: Based on the four project notebooks and their saved outputs; the notebooks have not been rerun for this report. Salary findings remain provisional because of the currency-conversion issue described in Section 8. Bracketed figure notes identify potential images and their sources; figures are not yet embedded.*

## 1. Introduction

### a) Problem statement and background

AI-related job titles can overlap even when the positions require different technical skills. This project investigated whether existing specialization labels and skill requirements provided useful ways to interpret those titles. The exploratory analysis addressed five questions: how postings were distributed across job characteristics; how compensation differed across groups; how titles related to job requirements; which skills distinguished specializations; and whether employer characteristics or work arrangements were associated with applicant volume.

### b) Project goal

The predictive stage extended that analysis by estimating salary midpoints from education, experience, and selected technical skills. This was a regression problem because the target was numeric. The project analyzed existing categories rather than training a classifier or using clustering to discover new job categories.

The intended audience includes job seekers trying to interpret role requirements and analysts comparing the characteristics of AI positions. A useful outcome is a clearer account of which skills distinguish specializations and how well selected posting attributes predict the advertised salary midpoint within this dataset.

### c) Overview of findings

The source dataset contained 51,932 postings and 22 variables. After initial cleaning and filtering by country labels, the analysis used 8,766 postings labeled as being in the United States or Canada. Machine Learning was the largest specialization, mid-level positions were the most common experience category, and remote or hybrid arrangements accounted for approximately 80% of postings. Technical skill profiles provided clear distinctions among specializations, including Computer Vision, Natural Language Processing, Generative AI, and MLOps.

In the saved modeling results, Random Forest performed best, with a test mean absolute error of $9,250 and an R² of 0.945. Linear Regression performed similarly, with a mean absolute error of $9,478 and an R² of 0.938. Both substantially outperformed a model that predicted the mean training salary for every posting. Experience-related features dominated the fitted models. However, Canadian salaries were converted in the wrong direction during wrangling, so these metrics describe the existing processed targets and must be recalculated before they can support conclusions about USD salary prediction.

## 2. Dataset

The wrangling notebook obtained the AI Jobs Dataset 2026 through KaggleHub using the identifier `m0sm71/ai-jobs-dataset-2026`. It loaded both an organized dataset and a raw postings file, then used the organized dataset for subsequent analysis. Its 22 variables included job title, employer characteristics, location, country, work arrangement, salary range, experience level, education, skills, specialization, and applicant count.

| Attribute group | Examples | Role in the project |
| --- | --- | --- |
| Job identity and specialization | Job title, AI specialization | Describe and compare categories |
| Employer characteristics | Industry, company size | Describe employer representation |
| Geography and work arrangement | Country, location, remote/hybrid/on-site | Define scope and compare arrangements |
| Qualifications | Education, experience level, years required, skills | Describe requirements and construct predictors |
| Compensation | Advertised salary range | Construct the salary-midpoint target |
| Applicant activity | Number of applicants | Compare applicant distributions |

The wrangling stage produced both a globally cleaned file and a country-filtered file. The exploratory, preprocessing, and modeling notebooks used the country-filtered version. Later title and skill comparisons used a smaller subset, while modeling retained the full country-filtered sample.

## 3. Data Cleaning and Wrangling

### a) Completeness and category consistency

Initial checks found no missing values in the organized dataset and no exact duplicate rows. These checks established completeness at the table level, but did not establish that every field was accurate or that different rows represented distinct underlying vacancies. Unique URLs and collection timestamps can prevent otherwise similar postings from appearing as exact duplicates.

Three industry categories containing one posting each were excluded, leaving 51,929 rows. Company-size labels were standardized by combining two nearly equivalent descriptions of the 1,000–5,000 employee range. Education descriptions were also consolidated. In particular, the ambiguous requirement “Master's or PhD” was assigned to a master's category, a simplification that should be considered when interpreting education features.

### b) Geographic scope

The country filter retained the United States, Canada, and Mexico, but no Mexican records were present. The resulting dataset contained 4,370 postings labeled United States and 4,396 labeled Canada. Therefore, the geographic coverage was limited to the two represented countries, subject to the accuracy of those labels.

[Insert Figure 1 here: Dataset preparation and analysis workflow. Create a flow diagram using the counts recorded in data_wrangling.ipynb, eda.ipynb, and data_preprocessing.ipynb: 51,932 source rows -> 51,929 after industry exclusions -> 8,766 after country filtering. Branch into 8,741 rows for popular-title analysis and 8,766 rows for modeling, then show the 7,012/1,754 train/test split.]

*Figure 1. Data preparation stages and the different samples used for detailed exploratory analysis and modeling.*

### c) Salary target

Salary ranges were parsed into lower and upper bounds. The prediction target was the arithmetic midpoint of those bounds. Although the exploratory notebook called this quantity “Median Salary,” it is a range midpoint, not the median of observed employee salaries. It also does not measure accepted offers or total compensation.

Canadian salary ranges were intended to be expressed in USD before comparisons and modeling. Review of the conversion code identified division by the stated CAD-to-USD rate where multiplication was required. Existing salary outputs are retained here as provisional results; Section 8 explains the correction needed before final submission.

## 4. Exploratory Data Analysis and Initial Findings

### a) Distribution of job postings

Machine Learning accounted for 3,245 postings, followed by MLOps with 1,389 and Generative AI with 1,318. NLP and Computer Vision contributed 961 and 945 postings, respectively, while Deep Learning and Data Science had 485 and 423. These figures describe the composition of this dataset and should not be interpreted as population estimates of demand across the AI labor market.

Mid-level positions represented 4,399 postings, or approximately 50.2% of the sample. Senior roles accounted for 2,641 postings and entry-level roles for 1,726. Remote positions were the largest work-arrangement group, with 3,959 postings, followed by 3,035 hybrid and 1,772 on-site postings. Full-time employment accounted for 7,465 postings, or approximately 85.2% of the sample. Counts across the ten retained industries were relatively similar.

[Insert Figure 2 here: Distribution of AI specializations, experience levels, and work arrangements. Use the corresponding three bar charts in eda.ipynb under "Tackling Question 1"; arrange them as panels A-C.]

*Figure 2. Composition of the 8,766 country-filtered postings by specialization, experience level, and work arrangement.*

### b) Compensation patterns

The notebook's initial salary comparisons suggested a stronger relationship with experience level than with specialization or job title. These were exploratory comparisons rather than causal tests. All salary comparisons need to be revisited after correcting currency conversion.

[Insert Figure 3 here: Salary midpoint distribution and salary by experience level. Use the histogram and experience-level box plot in eda.ipynb under "Tackling Question 2". Regenerate both after correcting CAD-to-USD conversion, and replace "Median Salary" with "Salary midpoint" in the labels.]

*Figure 3. Distribution of advertised salary midpoints and comparison across experience levels. Final USD values require corrected currency conversion.*

## 5. In-Depth Analysis

### a) Job titles and technical skills

For detailed title and skill comparisons, the analysis focused on titles with more than 100 postings. This subset contained 19 titles and 8,741 rows, excluding 25 rows from the full country-filtered sample. The restriction made comparisons less sensitive to very small groups, but it also excluded uncommon titles that may still represent meaningful roles.

The recorded skill-frequency analysis identified Pandas as the most common skill, appearing in approximately 46.0% of this subset. Several traditional machine-learning tools and methods appeared in approximately 39–40% of postings, including LightGBM, Scikit-learn, Random Forest, Regression, XGBoost, NumPy, Supervised Learning, and Feature Engineering. PyTorch appeared in approximately 22.1%, while several MLOps tools appeared in approximately 19%.

[Insert Figure 4 here: Most frequently requested technical skills. Create a horizontal bar chart from top_15_skills or skill_division_df in eda.ipynb under "Tackling Question 4". These values currently appear as a table/text output; use exact skill-token matching when refreshing the counts.]

*Figure 4. Recorded prevalence of the fifteen most common skills among the 8,741 postings with titles occurring more than 100 times.*

Overall frequency did not capture how strongly skills distinguished individual specializations. The analysis therefore calculated skill prevalence within each specialization and subtracted prevalence across the full popular-title subset. The resulting values were percentage-point differences, not percentage increases.

| Specialization | Examples of distinguishing skills in the analysis |
| --- | --- |
| Computer Vision | OCR, ResNet, OpenCV, YOLO, image processing |
| Data Science | Matplotlib, Statsmodels, SQL, A/B testing, data visualization |
| Deep Learning | Backpropagation, CNNs, CUDA, neural networks, Keras |
| Generative AI | Vector databases, LangChain, retrieval-augmented generation, fine-tuning, OpenAI API |
| MLOps | Git, model monitoring, Kubeflow, Kubernetes, Terraform |
| Machine Learning | Feature engineering, supervised learning, NumPy, XGBoost, regression |
| NLP | GPT, BERT, spaCy, tokenization, Transformers |

These patterns support interpreting the dataset's specialization labels as distinct technical skill profiles. A skill's overall popularity and its usefulness for identifying a particular specialization are different properties. For example, a specialized tool can be uncommon across the full sample but highly characteristic of one group.

[Insert Figure 5 here: Distinguishing skills by AI specialization. Use the "Distinguishing Skills by Specialization" heatmap in eda.ipynb. For readability, display a selection of distinguishing skills or divide the full heatmap into panels; retain the zero-centered color scale.]

*Figure 5. Difference, in percentage points, between skill prevalence within each specialization and prevalence across the popular-title subset.*

### b) Industry, work arrangement, and applicant volume

Within the popular-title subset, a chi-square test comparing industry and specialization produced a statistic of 61.93 and a p-value of 0.2141. A second test comparing specialization and work arrangement produced a statistic of 8.71 and a p-value of 0.7279. Neither test provided sufficient evidence to reject independence at a conventional 0.05 significance level. These results do not prove that the variables are unrelated.

[Insert Figure 6 here: Specialization across industries and work arrangements. Use the "Industry Distribution by AI Specialization" heatmap and "Work Arrangement by AI Specialization" stacked bar chart in eda.ipynb under "Tackling Question 5". The heatmap percentages sum within industry; the bar-chart percentages sum within specialization.]

*Figure 6. Specialization shares within industries and work-arrangement shares within specializations in the popular-title subset.*

Applicant counts showed substantial overlap across specializations, industries, work arrangements, and experience levels. Median counts by specialization ranged from 212 for Generative AI to 238 for Data Science, while within-group standard deviations were approximately 128–132 applicants. The descriptive results suggested that differences within groups were much larger than differences between their typical applicant counts. The analysis did not establish causal effects or build an applicant-volume prediction model.

[Insert Figure 7 here: Applicant counts by AI specialization. Use the applicant-count box plot immediately after applicants_by_specialization in eda.ipynb.]

*Figure 7. Applicant-count distributions overlap substantially across specializations in the popular-title subset.*

## 6. Preprocessing and Model Selection

### a) Feature engineering

The modeling dataset retained all 8,766 country-filtered postings. It contained 22 predictors: one normalized years-of-experience feature, three education indicators, three experience-level indicators, and fifteen binary skill indicators. Required experience ranged from zero to twelve years and was scaled to the interval from zero to one. Education was represented by bachelor's, master's, and PhD categories. Skill indicators used case-insensitive matching of complete comma-separated items, avoiding partial-name matches.

These feature choices defined the information available to the models. Hyperparameters, in contrast, controlled how each model learned from that information. For example, the number of selected skills was fixed during preprocessing, while tree depth was varied during Random Forest tuning. No additional scaling step was included in the model search. Linear Regression used 20 predictors after removing two reference-category indicators; Random Forest retained all 22.

### b) Training and evaluation approach

The data were divided into 7,012 training rows and 1,754 test rows using an 80/20 random split with a fixed seed of 42. Saved row identifiers kept predictors aligned with their salary targets. Five-fold shuffled cross-validation on the training set guided hyperparameter selection, and both model families used the same fold configuration.

Mean absolute error (MAE) was the primary evaluation metric because it expresses the average absolute prediction error in salary units. Root mean squared error (RMSE) provided a measure that gives greater weight to large errors. R² measured performance relative to variation in the test targets. A mean-predicting DummyRegressor provided a simple reference for assessing whether the job features improved prediction.

### c) Hyperparameter tuning strategy

Hyperparameters are settings specified before model fitting, such as whether Linear Regression includes an intercept or how deep a Random Forest tree can grow. They differ from learned parameters, such as regression coefficients and tree split thresholds, which are estimated from the training data. This project compared a small set of hyperparameter combinations for each model and selected the combination with the lowest average validation MAE.

For each candidate configuration, five-fold cross-validation fitted the model on four training folds and evaluated it on the remaining fold, repeating the process until every fold had served as validation data. Both searches used `KFold(n_splits=5, shuffle=True, random_state=42)`, providing matching validation partitions. The scoring setting was `neg_mean_absolute_error`: the search maximized the negative score, which is equivalent to minimizing MAE. Scores were converted back to positive error values for reporting.

Linear Regression used `RandomizedSearchCV` with `n_iter=4`. Because its search space contained exactly four combinations, the search evaluated every combination rather than sampling only part of the space. Random Forest used `GridSearchCV` to evaluate all eight combinations in its search space. This required 20 cross-validation fits for Linear Regression and 40 for Random Forest, followed by one refit of each selected configuration on all 7,012 training rows. The 1,754 test rows were excluded from these searches and used afterward for the recorded model comparison.

The fixed random seeds supported reproducibility; they were not tuned for better scores. Likewise, `n_jobs` controlled computation rather than model quality. Both searches used `n_jobs=-1` to parallelize candidate evaluation, while each forest used `n_jobs=1` to avoid parallelizing at both levels. Training scores were retained alongside validation scores to assess overfitting. The validation standard deviations describe variation across these five folds, not confidence intervals, and the winning validation scores are not independent evaluations because they also guided selection. The earlier preprocessing limitations are discussed in Section 8.

## 7. Modeling Results and Interpretation

### a) Linear Regression

Linear Regression provided an interpretable starting point. Bachelor's education and entry-level experience were omitted as reference categories, leaving 20 predictors. The same predictor columns were used for all four configurations. The search varied two settings:

| Hyperparameter | Values tested | What it controls | Selected value |
| --- | --- | --- | --- |
| `fit_intercept` | `True`, `False` | Whether the model learns a constant starting value in addition to feature coefficients. With `False`, that constant is fixed at zero. | `True` |
| `positive` | `False`, `True` | Whether coefficients can have either sign or must be nonnegative. This restriction applies to coefficients, not the intercept. | `True` |

Keeping an intercept allowed the model to represent a starting salary for the reference categories. Turning it off imposed a different constraint on the same feature matrix, and those configurations performed worse in validation. Allowing either sign for coefficients represented the original unconstrained model; restricting them to nonnegative values tested whether a simpler set of permitted relationships improved validation performance. This restriction does not guarantee positive salary predictions or establish that every feature increases salary.

The selected configuration was `fit_intercept=True` and `positive=True`. Its mean cross-validation MAE was $9,735.49, with a fold standard deviation of $157.62, compared with $9,756.88 for the original settings (`fit_intercept=True`, `positive=False`). Tuning therefore reduced validation MAE by only $21.39, approximately 0.22%. This improvement was small relative to the variation between folds. The selected model's mean training-fold MAE was $9,719.57, close to its validation MAE, providing little indication of a large training-to-validation performance gap under this evaluation.

### b) Random Forest

Random Forest was used to capture nonlinear relationships and interactions by averaging predictions from multiple decision trees. Its search varied three hyperparameters, each with two candidate values, producing eight combinations:

| Hyperparameter | Values tested | What it controls | Selected value |
| --- | --- | --- | --- |
| `max_depth` | `8`, `None` | Maximum depth of each tree. `None` removes the depth limit, although other stopping rules still apply. | `8` |
| `min_samples_leaf` | `1`, `5` | Minimum number of training samples required in a leaf, the final group used to produce a tree prediction. Larger leaves discourage fitting very small groups. | `5` |
| `max_features` | `0.5`, `1.0` | Fraction of predictors considered when searching for a split. The candidates correspond to half or all of the 22 features. | `0.5` |
| `n_estimators` | Fixed at `150`; not searched | Number of trees whose predictions are averaged. This value kept search time manageable. | `150` |

The tree count was a fixed hyperparameter, so the experiment did not establish that 150 trees was optimal. The forest also used `random_state=42` for repeatable random sampling. Limiting depth and increasing leaf size restricted how closely individual trees could fit local details. Considering only half the predictors at each split introduced more variation among trees, which can reduce their tendency to make similar errors.

The selected configuration used `max_depth=8`, `min_samples_leaf=5`, and `max_features=0.5`. Its mean cross-validation MAE was $9,559.79, with a standard deviation of $110.46. Its mean training-fold MAE was $9,258.59, giving a training-to-validation gap of about $301. In contrast, forests with no depth limit and one-sample minimum leaves had training MAEs of approximately $8,100 but validation MAEs of approximately $10,100. Their closer fit to training rows did not translate into better validation predictions.

| Configuration | Mean training MAE | Mean validation MAE |
| --- | ---: | ---: |
| Selected: depth 8, leaf minimum 5, half the features | $9,258.59 | $9,559.79 |
| Depth 8, leaf minimum 5, all features | $9,215.73 | $9,581.74 |
| No depth limit, leaf minimum 1, half the features | $8,097.97 | $10,087.26 |
| No depth limit, leaf minimum 1, all features | $8,102.55 | $10,099.54 |

This comparison illustrates why selection relied on validation error rather than training error. The selected configuration was best among the eight tested combinations; the limited search does not establish that it is the best possible Random Forest configuration. All tuning scores reported here remain provisional pending correction of the salary targets.

[Insert Figure 8 here: Training and validation errors across Random Forest configurations. Create a grouped bar chart from forest_validation_results in data_modeling.ipynb. Label each configuration by tree depth, minimum leaf size, and max_features. Regenerate after correcting the salary targets.]

*Figure 8. Cross-validation training and validation MAE for the eight forest configurations. Larger gaps indicate poorer transfer from training rows to validation rows.*

### c) Model comparison

After tuning, each search refitted its selected model on the full training set. The following results compare those fitted models on the same held-out test rows; they are distinct from the validation scores used to select hyperparameters. The mean baseline was fitted using `strategy='mean'` and was not tuned.

| Model | Test MAE | Test RMSE | Test R² |
| --- | ---: | ---: | ---: |
| Mean baseline | $40,567.00 | $48,596.96 | -0.0016 |
| Linear Regression | $9,478.15 | $12,117.90 | 0.9377 |
| Random Forest | $9,250.28 | $11,403.42 | 0.9449 |

*These dollar-denominated values reproduce the current notebook outputs. They are not validated USD performance estimates because the underlying Canadian salary targets were converted incorrectly.*

Random Forest reduced test MAE by approximately $228, or 2.4%, relative to Linear Regression. Its RMSE was approximately 5.9% lower. It was therefore the strongest model in the recorded comparison, although the gain over Linear Regression was modest and was observed on a single test split.

### d) Model interpretation

Experience was the dominant predictor group. In the Random Forest, normalized years of experience and the three experience-level indicators together accounted for approximately 96.8% of impurity-based feature importance. Experience-level indicators also had the largest Linear Regression coefficients. These results describe how the fitted models used the available features; they do not show that increasing experience causes a specific salary increase. Correlated predictors can share importance, and nonnegative coefficient constraints can force some linear coefficients to zero.

[Insert Figure 9 here: Random Forest feature importance. Create a horizontal bar chart from forest_importances in data_modeling.ipynb; the notebook currently displays these scores as a table. Regenerate after correcting salary targets.]

*Figure 9. Impurity-based importance of predictors in the fitted Random Forest. These scores describe model reliance, not causal salary effects.*

### e) Prediction diagnostics

The modeling notebook includes actual-versus-predicted plots and residual plots for both models. In an actual-versus-predicted plot, proximity to the diagonal represents smaller prediction error. Residuals are the actual midpoint minus the predicted midpoint, so positive residuals indicate underprediction. These plots provide a way to examine errors that aggregate metrics can hide, including systematic underprediction at higher salaries or changes in error spread. Their final interpretation should follow regeneration of the corrected salary targets.

[Insert Figure 10 here: Actual versus predicted salary midpoints. Place the Linear Regression and Random Forest actual-versus-predicted plots from data_modeling.ipynb side by side. Use the same axis limits for comparison and regenerate after correcting the targets.]

*Figure 10. Comparison of test-set predictions with observed salary midpoints. The diagonal represents perfect predictions.*

[Insert Figure 11 here: Residual plots for Linear Regression and Random Forest. Use "Linear Regression Residuals" and "Random Forest Residuals" from data_modeling.ipynb. Use comparable axes and regenerate after correcting the targets.]

*Figure 11. Actual minus predicted salary midpoint against predicted midpoint for each model. The horizontal line marks zero error.*

## 8. Limitations

The most consequential issue is currency conversion. The wrangling notebook describes a rate of 0.7158 USD per CAD but divides Canadian salaries by that value. Under the stated rate, the calculation should multiply CAD amounts by 0.7158. For example, C$100,000 would become $71,580 under that assumption, whereas the existing code produces approximately $139,704. The appropriateness of the chosen 2025 rate for the posting dates also needs review. Correcting this step requires regenerating the processed salary data and rerunning salary analysis, preprocessing, and modeling. The current model ranking may change afterward.

Geographic labels also require validation. Displayed records include locations in Malta and Qatar paired with a United States country label. Filtering on the country field alone therefore does not guarantee a geographically accurate North American sample. Similarly, repeated descriptions and unusually regular skill patterns warrant checking how the source dataset was collected and prepared. The notebooks alone do not establish whether every field reflects an independently observed posting or whether some attributes were generated or filled in.

Some preprocessing decisions used information from the full dataset before the train/test split. The top skills were selected during exploratory analysis, and the normalization bounds were calculated before splitting. Although these steps did not directly use salary targets, a stricter evaluation would learn data-dependent preprocessing from training data only and repeat it within cross-validation folds. Exploratory skill frequencies also used substring and regular-expression matching; recalculating them using the exact-token method already used for modeling would make the workflow more consistent.

## 9. Takeaways

Technical skill profiles provided the clearest descriptive distinctions among the existing specializations. For interpreting a posting, the combination of specialization and required tools was more informative about its technical focus than an overall ranking of common skills. The patterns were observed within this dataset and should be validated against independently sourced postings before being generalized.

Experience-related features dominated both salary models. Random Forest achieved the lowest recorded validation and test MAE, but its test improvement over Linear Regression was small. Linear Regression remains a useful comparison because its predictions were close and its coefficients are easier to describe. Neither model is ready to support salary estimates until the target values have been corrected and performance reassessed.

The exploratory analysis found limited evidence of differences in applicant volume across the examined groups, and the two chi-square tests did not establish associations between specialization and industry or work arrangement. These findings support cautious, dataset-specific interpretation rather than claims that these characteristics never matter.

## 10. Future Work

The first priority is to correct currency conversion, validate country labels against locations, and regenerate all downstream salary results. The report's salary figures, model comparison table, and conclusions about model performance should then be updated together.

The predictive feature set omitted country, location, industry, work arrangement, title, and specialization. After data correction, testing these variables could establish whether they add information beyond experience. An experience-only model would also help quantify the incremental value of education and skills. Permutation importance and errors broken down by country, experience level, and specialization would provide a more complete assessment than aggregate metrics alone.

Finally, random splitting does not directly test performance on future postings or unfamiliar employers. A temporal split or employer-grouped evaluation would address those questions. Because the existing test results have already informed model comparison, further development should use a fresh final holdout or a nested evaluation design.

## 11. Conclusion

The project provides a descriptive framework for interpreting AI jobs through specialization, skill requirements, and experience expectations. Its strongest descriptive finding is that existing specializations correspond to recognizable technical skill profiles. The saved models also show that experience-related features explain much of the variation in the currently processed salary targets, with Random Forest producing a small improvement over Linear Regression.

The work demonstrates an end-to-end workflow from data inspection and cleaning through feature engineering, tuning, and evaluation. Its salary conclusions remain provisional until currency conversion and geographic labels are corrected and the pipeline is rerun. Addressing those issues would make the final report more defensible and clarify how well its findings extend beyond this dataset.

## 12. References and Project Materials

- [Data wrangling notebook](../notebooks/data_wrangling.ipynb): problem statement, source data, cleaning, country filtering, and currency conversion.
- [Exploratory analysis notebook](../notebooks/eda.ipynb): distributions, title subsets, skill comparisons, chi-square tests, and applicant summaries.
- [Preprocessing notebook](../notebooks/data_preprocessing.ipynb): target construction, feature encoding, and train/test split.
- [Modeling notebook](../notebooks/data_modeling.ipynb): baseline, model searches, saved evaluation metrics, and feature interpretation.

The report structure was informed by Eunice Kim's *Bluebikes: recommendation of locations for new bike stations* milestone report (Bluebikes_Milestone.pdf) and Drew Adamski's *Final Report: NYC Water Quality Network Analysis* (Final_Rep_NYC_WQ.pdf), supplied as examples. The introduction, dataset, cleaning, and initial findings sequence follows the Bluebikes example; the in-depth analysis, modeling, takeaways, and future work sequence draws on the NYC example. Numbered figures are placed alongside the discussion they support, as in both reports. These documents serve as structural references, not as evidence for the AI-job findings.
