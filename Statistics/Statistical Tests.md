### **Parametric Tests (Assume Normal Distribution)**  
1. **t-Test (Student’s t-test)** – Determines if there is a significant difference between the means of two groups.  
   - **Independent t-test** – Compares the means of two independent (non-paired) groups (assumes equal variance).  
   - **Welch’s t-test** – Compares the means of two independent (non-paired) groups without assuming equal variance (more reliable when variances are unequal).  
   - **Paired t-test** – Compares the means of two related (paired) groups (dependent samples) by analyzing differences within subjects (e.g., before-and-after measurements on the same individuals).  
   - **One-Tailed t-Test** – A variation of the t-test that tests if the mean of one group is significantly greater than or less than the other in a specific direction (e.g., Group A > Group B). Used when a directional hypothesis is specified.  
   - **Two-Tailed t-Test** – Tests if the means of two groups are significantly different in either direction (greater than or less than). Used when no specific direction is hypothesized.  

2. **ANOVA (Analysis of Variance)** – Determines whether there are statistically significant differences between the means of three or more groups.  
   - **One-way ANOVA** – Tests differences in means among multiple independent (non-paired) groups based on a single independent variable.  
   - **Two-way ANOVA** – Examines the effect of two independent variables on a dependent variable, including possible interaction effects, typically for independent (non-paired) groups.  
   - **Repeated Measures ANOVA** – Compares means from the same subjects (paired) under different conditions or time points (reduces variability by accounting for within-subject differences).  
   - **Post-Hoc Tests**: When ANOVA shows significance, multiple comparisons are often adjusted using methods like Bonferroni, Holm, or Sidak to control for Type I errors (see below).  

3. **Regression Analysis** – Evaluates relationships between independent and dependent variables, often used for prediction.  
   - **Simple Linear Regression** – Models the relationship between a single independent variable and a continuous dependent variable (non-paired data typically assumed).  
   - **Multiple Regression** – Analyzes the impact of two or more independent variables on a dependent variable (non-paired data).  
   - **Logistic Regression** – Used when the dependent variable is categorical (e.g., binary outcomes like success/failure), typically with non-paired data.  

4. **Chi-Square Test (χ² Test)** – Assesses the association between categorical variables by comparing observed and expected frequencies in contingency tables.  
   - **Chi-Square Goodness-of-Fit Test** – Determines if a sample distribution matches an expected distribution (one-tailed in nature, testing deviation in a specific direction).  
   - **Chi-Square Test for Independence** – Evaluates relationships between two categorical variables (typically non-paired data).  

5. **F-Test** – Used to compare variances between groups, often as a preliminary step in ANOVA or regression analysis. Generally **not appropriate** for testing differences between group means.  

6. **Cohen's d** – A measure of effect size used to determine the magnitude of the difference between two groups, commonly used in t-tests (paired or non-paired). It is calculated as the difference between the means of two groups divided by the pooled standard deviation.  
   - **Small Effect**: d = 0.2  
   - **Medium Effect**: d = 0.5  
   - **Large Effect**: d = 0.8  

7. **Hotelling's T-squared** – A multivariate generalization of the t-test, used to compare the mean vectors of two groups (paired or non-paired) on multiple variables, assuming multivariate normality.  

---

### **Non-Parametric Tests (Used When Data is Not Normally Distributed)**  
1. **Mann-Whitney U Test (Wilcoxon Rank-Sum Test)** – Compares the distributions of two independent (non-paired) groups; a non-parametric alternative to the independent t-test. Can be one-tailed or two-tailed depending on the hypothesis.  

2. **Wilcoxon Signed-Rank Test** – Compares two related (paired) groups by analyzing differences in ranks, serving as a non-parametric alternative to the paired t-test. Can be one-tailed or two-tailed.  

3. **Kruskal-Wallis H Test** – A non-parametric version of one-way ANOVA, comparing the medians of three or more independent (non-paired) groups. Typically two-tailed.  

4. **Friedman Test** – A non-parametric alternative to repeated measures ANOVA, used for analyzing repeated measures (paired) data when normality is not assumed. Typically two-tailed.  

5. **Spearman’s Rank Correlation** – Measures the strength and direction of the relationship between two non-normally distributed or ordinal variables (paired data).  

6. **Kendall’s Tau** – Similar to Spearman’s correlation but better suited for small sample sizes and tied ranks (paired data).  

---

### **Normality & Distribution Tests**  
1. **Kolmogorov-Smirnov (K-S) Test** – Compares a sample’s distribution to a reference distribution (e.g., normal, uniform). Can be one-tailed or two-tailed depending on the hypothesis.  

2. **Shapiro-Wilk Test** – Specifically tests for normality in small to moderate-sized datasets (more powerful than K-S for normality assessment). Typically two-tailed.  

---

### **Other Significance Tests**  
1. **Z-Test** – Similar to the t-test but used when the sample size is large (n > 30) and population variance is known. Can be one-tailed or two-tailed, paired or non-paired.  

2. **Log-Rank Test** – Used in survival analysis to compare survival distributions between two or more groups (typically non-paired, two-tailed).  

3. **McNemar’s Test** – Tests for changes in paired categorical data, commonly used for before-and-after studies or matched case-control studies (paired, typically two-tailed).  

---

### **Post-Hoc correction methods**


The **Bonferroni**, **Holm**, and **Sidak** methods are **post hoc correction methods** used after performing multiple statistical tests, especially ANOVA, to control the **family-wise error rate (FWER)**—the probability of making at least one Type I error (false positive) when conducting multiple comparisons.

### 1. **Bonferroni Correction**
- **Simple and conservative**.
- Divide the significance level (alpha) by the number of comparisons:
  $$
  \alpha' = \frac{\alpha}{n}
  $$
- Example: If alpha is 0.05 and you have 5 comparisons, each individual test must be significant at 0.01 (0.05/5) to be considered statistically significant.
- **Pros:** Easy to compute, controls FWER well.  
- **Cons:** Very conservative, especially with many comparisons—may miss real effects (Type II errors).

### 2. **Holm’s Method (Holm-Bonferroni)**
- **Stepwise and less conservative** than Bonferroni.
- Procedure:
  1. Rank the p-values from smallest to largest.
  2. Compare the smallest p-value to $$\frac{\alpha}{n}$$, the second smallest to $$\frac{\alpha}{n-1}$$, and so on.
  3. Reject null hypotheses sequentially until you reach a p-value that is **not** significant.
- **Pros:** More power than Bonferroni while still controlling FWER.

### 3. **Sidak Correction**
- Slightly less conservative than Bonferroni, assuming **independent tests**.
- Formula:
  $$
  \alpha' = 1 - (1 - \alpha)^{1/n}
  $$
- There’s also a **Holm-Sidak** version, which uses this adjusted alpha at each step.
- **Pros:** More accurate than Bonferroni for independent tests.

### 4. **Tukey’s HSD (Honestly Significant Difference)**
- Designed for **pairwise comparisons** after ANOVA.
- Uses the **studentized range distribution**.
- Assumes equal variances and sample sizes.
- **Best for**: Comparing all pairs of group means.
- **Pros:** More power than Bonferroni for pairwise tests.  
- **Cons:** Not ideal with unequal variances or sample sizes.

### 5. **Holm-Sidak Method**
- A **step-down Sidak** method: combines Holm's ranking with Sidak-adjusted alpha.
- Procedure:
  1. Rank p-values.
  2. Compare each p-value to:
     $$
     \alpha_i = 1 - (1 - \alpha)^{1/(n - i + 1)}
     $$
  3. Stop when a p-value is not significant.
- **Pros:** Higher power than Holm-Bonferroni under independence.

### 6. **Scheffé’s Method**
- Very **conservative**, even more than Bonferroni.
- Allows for **any linear contrast**, not just pairwise.
- Based on the **F-distribution**.
- **Best for**: Exploratory comparisons and complex contrasts.
- **Pros:** Robust and flexible.  
- **Cons:** Low power—needs large effects to detect significance.

### 7. **Dunnett’s Test**
- Designed for comparing **multiple groups to a single control**.
- Controls FWER specifically for these one-to-many comparisons.
- **Best for**: Clinical or experimental studies with a control group.
- **Pros:** More powerful than Bonferroni in this setup.  
- **Cons:** Not for comparing all pairs—only treatment vs. control.

---

### 📊 Summary Table

| Method         | Conservative? | Assumes Independence? | Type                  | Ideal Use Case                                |
|----------------|----------------|------------------------|-----------------------|------------------------------------------------|
| **Bonferroni**     | Yes            | No                     | Simple                | General multiple comparisons                  |
| **Holm**           | Less           | No                     | Stepwise              | General, more power than Bonferroni           |
| **Sidak**          | Less           | Yes                    | Simple                | Independent tests                             |
| **Holm-Sidak**     | Less           | Yes                    | Stepwise              | Independent tests, more power                 |
| **Tukey’s HSD**    | Moderate       | Yes (equal variances)  | Pairwise              | All pairwise comparisons post-ANOVA           |
| **Scheffé**         | Very           | No                     | Contrast-based        | Any linear contrast, exploratory testing      |
| **Dunnett’s Test** | Moderate       | Yes                    | One-vs-all            | Comparisons of groups to a single control     |


---

### **Key Concepts**  
- **One-Tailed vs. Two-Tailed**:  
  - **One-Tailed**: Tests a directional hypothesis (e.g., Group A > Group B). P-value is calculated for one side of the distribution, providing more power if the direction is correctly specified.  
  - **Two-Tailed**: Tests a non-directional hypothesis (e.g., Group A ≠ Group B). P-value is split across both tails, more conservative but appropriate when direction is uncertain.  
- **Paired vs. Non-Paired**:  
  - **Paired**: Data points are related (e.g., same subjects measured twice, matched pairs). Reduces variability by focusing on within-subject differences.  
  - **Non-Paired (Independent)**: Data points are unrelated (e.g., two separate groups). Compares between-group differences, assuming independence.  

--- 

