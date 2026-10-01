**Slide 1 — Problem statement**

- Introduce the challenge: AI job titles have expanded quickly, and similar titles can describe different responsibilities.
- Explain the business consequence: unclear roles make hiring requirements, experience expectations, and compensation harder to compare.
- State your objective: identify meaningful role categories and explain their distinguishing characteristics.
- Clarify that although the project considers North America, the analyzed sample contains U.S. and Canadian postings.

**Transition:** “The analysis points to several findings that can help clarify these roles.”

**Slide 2 — Executive findings**

- Specializations have recognizable skill profiles that can support clearer job descriptions.
- Experience-related features dominate the current salary models.
- The strongest immediate application is understanding role requirements.
- Salary findings remain provisional until currency conversion and country labels are corrected.

**Transition:** “Before discussing the implications, here is the scope of the evidence.”

**Slide 3 — Evidence behind the findings**

- The main analysis uses **8,766 job postings**, split almost equally between the United States and Canada.
- The source includes **22 variables** covering job characteristics, skills, experience, and compensation.
- Detailed skill comparisons focus on frequently listed titles to avoid relying heavily on very small groups.
- These findings describe the sample; they do not establish the size or composition of the entire AI labor market.

**Slide 4 — Specialization is a useful starting point for hiring**

- Different AI roles require different capabilities, even when their titles sound similar.
- Compare a few examples: Machine Learning focuses on model development, Generative AI includes retrieval and fine-tuning, and MLOps emphasizes deployment and monitoring.
- For a business, the first question should be what work the role needs to accomplish.
- The specialization profiles can help hiring teams translate that work into relevant skill requirements.

**Transition:** “There is also a shared foundation across many of these roles.”

**Slide 5 — Common tools support a shared skills foundation**

- Pandas appears in approximately **46%** of the popular-title sample.
- Several traditional machine-learning tools and methods appear in approximately **39–40%**.
- Common tools can help identify broad training needs.
- Specialized capabilities help distinguish candidates for particular responsibilities.
- Posting frequency indicates what employers list, rather than proving how proficient applicants are.

**Slide 6 — The sample favors mid-level, remote, full-time roles**

- Mid-level positions account for about half the sample.
- Remote work is the largest work-arrangement category, and full-time employment dominates.
- These patterns provide context for recruiting and workforce planning.
- Before using them to set company policy, compare them with the organization’s own location, responsibilities, and relevant talent pool.

**Slide 7 — Applicant counts offer limited differentiation**

- Median applicant counts range from **212 to 238** across specializations.
- Variation within each group is much larger than the differences between group medians.
- Specialization alone offers limited guidance about expected applicant volume in this sample.
- Applicant counts do not measure candidate quality, hiring difficulty, or the number of qualified candidates.

**Transition:** “The compensation analysis showed a much stronger pattern around experience.”

**Slide 8 — Experience carries most of the current salary signal**

- Experience-related features account for approximately **96.8% of Random Forest’s feature importance**.
- Explain that this percentage describes how the model uses its predictors; it is not the percentage of salary determined by experience.
- The business implication is that consistent seniority and experience definitions deserve attention in role design.
- This remains a provisional finding and does not establish a causal relationship between experience and pay.

**Slide 9 — The salary model is an initial analytical result**

- You compared a straightforward Linear Regression model with a Random Forest model that can capture more complex patterns.
- Random Forest’s average absolute test error was approximately **$9,250**, compared with approximately **$9,478** for Linear Regression.
- The improvement was about **2.4%**, so the more complex model provided a modest advantage.
- These errors concern advertised salary-range midpoints, rather than actual employee compensation.
- The current results require data correction and validation before supporting compensation decisions.

**Slide 10 — Business use requires corrected salary data**

- Separate the descriptive findings from the applications requiring more evidence.
- Skill profiles can support discussions about job requirements, training, and role definitions.
- Compensation benchmarks and forecasts require corrected salary data.
- Market-wide demand estimates and predictions for future postings require broader validation.
- This distinction helps stakeholders understand which decisions the project can currently inform.

**Transition:** “With those boundaries established, several groups could benefit from this information.”

**Slide 11 — Who can benefit from this information?**

- **HR and recruiting teams:** clarify job descriptions and identify skills relevant to each specialization.
- **Executives and workforce planners:** compare role needs and identify where further workforce analysis would help.
- **Academic leaders and educators:** use specialization profiles to inform course topics and training priorities.
- **Job seekers and career advisers:** compare roles and identify skills to develop.
- Compensation-related benefits depend on correcting the data and rerunning the analysis.

**Slide 12 — Next steps for a defensible business application**

- First, correct currency conversion and validate country labels against posting locations.
- Then rerun the salary analysis and update the results together.
- Test whether location, title, specialization, and industry improve predictions.
- Examine errors within relevant workforce segments, rather than relying only on one overall score.
- Evaluate future postings or unfamiliar employers before using predictions in planning.

**Slide 13 — Closing perspective**

- The strongest immediate opportunity is clearer AI role definitions.
- The specialization findings can support more focused conversations about hiring requirements and training.
- Reliable compensation planning is a potential next application after correction and validation.
- Close with a business question: **“Which workforce decision would benefit most from better AI role definitions?”**

**Slide 14 — Appendix: Complete specialization profiles**

- Use this slide if someone asks about a specialization not covered in the main presentation.
- Explain that the listed skills appeared more frequently within their specialization relative to the comparison sample.
- Treat the examples as role signals, rather than an exhaustive hiring checklist.
- Discuss the profile most relevant to the audience’s business needs.

**Slide 15 — Appendix: Modeling and evaluation**

- Use this slide for questions about how you assessed the salary models.
- Explain that you reserved 20% of the data for testing and used cross-validation on the training data to select settings.
- Define average absolute error as the average size of a prediction miss in salary units.
- Emphasize that evaluation checks model performance on the processed data; it does not resolve inaccuracies in that data.
- Reiterate that the salary results remain provisional.