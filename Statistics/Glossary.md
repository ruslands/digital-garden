### **1. Mean (Average)**  
- **Definition**: The **mean** is the sum of all values in a dataset divided by the number of values in that dataset. It provides a measure of the **central tendency** of the data.
- **Formula**:  
  ```markdown
  Mean = (Σ₁ⁿ xᵢ) / n
  ```
  where \( xᵢ \) is each data point, and \( n \) is the total number of data points.

### **2. Median**  
- **Definition**: The **median** is the middle value of a dataset when the data is ordered from least to greatest. If the dataset has an even number of elements, the median is the average of the two middle values. The median is a measure of central tendency that is less affected by outliers or skewed data compared to the mean.
- **Formula**: 
  - For an odd number of values, the median is the middle value.
  - For an even number of values, the median is the average of the two middle values.
  
### **3. Variance**  
- **Definition**: **Variance** measures the spread or dispersion of a dataset. It quantifies how much the individual data points differ from the **mean**. A higher variance means the data points are more spread out from the mean, while a lower variance indicates that the data points are closer to the mean.
- **Formula**:  
```markdown
  Variance = Σ₁ⁿ (xᵢ - Mean)² / n
```
  where \( xᵢ \) represents each individual data point, and \( n \) is the number of data points in the dataset.

### **4. Standard Deviation** (related to Variance)
- **Definition**: The **standard deviation** is the square root of the variance and is also a measure of the spread of data. It provides a sense of the average distance between data points and the mean.
- **Formula**:
  ```markdown
  Standard Deviation = √Variance
  ```

---

### **5. Mode**
- **Definition**: The **mode** is the value that appears most frequently in a dataset. A dataset may have one mode (unimodal), more than one mode (bimodal or multimodal), or no mode if all values occur with the same frequency.
  
---

### **6. Range**
- **Definition**: The **range** is the difference between the largest and smallest values in a dataset. It gives a basic measure of the spread of data.
- **Formula**:  
  ```markdown
  Range = Maximum Value - Minimum Value
  ```

---

### **7. Skewness**
- **Definition**: **Skewness** measures the asymmetry of the distribution of data. 
   - **Positive skew** (right-skewed): The right tail is longer or fatter than the left tail.
   - **Negative skew** (left-skewed): The left tail is longer or fatter than the right tail.
   - **Zero skew**: The data is perfectly symmetrical.

---

### **8. Kurtosis**
- **Definition**: **Kurtosis** measures the "tailedness" or the sharpness of the peak of a data distribution. 
   - **High kurtosis** means more outliers and a sharper peak.
   - **Low kurtosis** indicates a flatter distribution with fewer outliers.
   - **Normal kurtosis** (Mesokurtic) refers to a normal distribution with moderate tails.

---

### **9. Confidence Interval (CI)**
- **Definition**: A **confidence interval** is a range of values that is used to estimate the true population parameter with a certain level of confidence (usually 95% or 99%). The wider the interval, the less precise the estimate, and vice versa.
  
---

### **10. P-value**
- **Definition**: The **p-value** is the probability of obtaining results at least as extreme as those observed, assuming the null hypothesis is true. A smaller p-value indicates stronger evidence against the null hypothesis.
   - **Interpretation**:  
     - If \( p < 0.05 \), reject the null hypothesis (statistically significant).
     - If \( p > 0.05 \), fail to reject the null hypothesis (not statistically significant).

---

### **11. Null Hypothesis (H₀)**
- **Definition**: The **null hypothesis** represents the default assumption that there is **no effect** or **no difference** between groups or variables being tested. The goal is to determine whether there is enough evidence to reject it in favor of the alternative hypothesis.
The **null hypothesis** (denoted as **H₀**) is a statement or assumption in statistical testing that there is **no effect** or **no difference** between the groups or variables being studied. It serves as the default assumption that any observed differences or relationships are due to **random chance** rather than a true effect.

#### In the context of your tests:
1. **Paired t-test**:
   - **Null Hypothesis (H₀)**: There is **no significant difference** between the "before" and "after" measurements (i.e., the mean difference is zero).
   - **Alternative Hypothesis (H₁)**: There **is a significant difference** between the "before" and "after" measurements (i.e., the mean difference is not zero).

2. **Wilcoxon Signed-Rank Test**:
   - **Null Hypothesis (H₀)**: The **distribution** of the "before" and "after" measurements is the same (i.e., no significant change between the paired observations).
   - **Alternative Hypothesis (H₁)**: There is a **significant change** in the distribution between the "before" and "after" measurements.

#### The Process:
- **Rejecting the Null Hypothesis**: If the p-value is **less than 0.05**, it suggests that the observed difference is **statistically significant** (not due to chance), and we **reject the null hypothesis**.
- **Failing to Reject the Null Hypothesis**: If the p-value is **greater than 0.05**, we **fail to reject the null hypothesis**, meaning that there is no significant difference between the groups (or the difference is likely due to chance).

In summary:
- **H₀ (Null Hypothesis)** = No effect or difference.
- **H₁ (Alternative Hypothesis)** = There is an effect or difference.
 that there is **no effect** or **no difference** between groups or variables being tested. The goal is to determine whether there is enough evidence to reject it in favor of the alternative hypothesis.

---

### **12. Alternative Hypothesis (H₁ or Ha)**
- **Definition**: The **alternative hypothesis** represents the hypothesis that there **is a significant effect** or difference. It is the hypothesis that researchers attempt to support through statistical evidence, in contrast to the null hypothesis.

---

### **13. Type I Error (α)**
- **Definition**: A **Type I error** occurs when the null hypothesis is **incorrectly rejected** (false positive). This means concluding that there is a significant effect or difference when there actually isn't.
   - The probability of making a Type I error is denoted by \( \alpha \) (commonly set at 0.05).

---

### **14. Type II Error (β)**
- **Definition**: A **Type II error** occurs when the null hypothesis is **incorrectly accepted** (false negative). This means concluding that there is no significant effect or difference when there actually is.
   - The probability of making a Type II error is denoted by \( \beta \).

---

### **15. Power of a Test**
- **Definition**: The **power** of a statistical test is the probability that the test will correctly reject the null hypothesis when it is false (i.e., the test's ability to detect an effect when there is one).
   - Power = \( 1 - \beta \)
   - A high power (usually ≥ 0.80) is desirable because it reduces the risk of Type II errors.

---

### **16. Sample Size**
- **Definition**: The **sample size** refers to the number of observations or data points in a study or experiment. A larger sample size generally increases the reliability of the results and the power of the statistical test.

---

### **17. Effect Size**
- **Definition**: **Effect size** is a measure of the **magnitude** of a difference or relationship. It quantifies the size of the effect, regardless of sample size.
   - Common measures of effect size include:
     - **Cohen's d**: Measures the difference between two means in terms of standard deviation.
     - **Pearson's r**: Measures the strength and direction of a linear relationship between two variables.
     - **Eta-squared (η²)**: Measures the proportion of variance explained by a factor in ANOVA.

---

### **18. Degrees of Freedom (df)**
- **Definition**: **Degrees of freedom** refer to the number of independent values or quantities that can vary in the analysis without violating any constraints. It is an important concept in many statistical tests like t-tests, ANOVA, and chi-square tests.

---

### **19. Sample vs Population**
- **Sample**: A subset of a population used for analysis. Inference from the sample is generalized to the larger population.
- **Population**: The entire set of individuals or items that you are studying or drawing conclusions about.


---

### **20. F Value**
- **Definition**: The F Value is the test statistic in ANOVA, calculated as the ratio of the variance **between** the time points (due to the effect of time) to the variance **within** the time points (due to individual differences and random error).
- **Formula**:
$$
F = \frac{\text{Mean Square Between (MSB)}}{\text{Mean Square Within (MSW)}}
$$
  - **Mean Square Between (MSB)**: Measures variability due to differences between the time points (e.g., `before`, `after`, `after_2`).
  - **Mean Square Within (MSW)**: Measures variability within subjects across time points, accounting for random error and individual differences.
- **Purpose**: The F Value tests the null hypothesis that the means of CPEPTID (or other measurements) are equal across all time points. A larger F Value suggests that the differences between time points are greater than would be expected by chance.
- **Example from CPEPTID**:
  - For CPEPTID, the F Value is **14.830537**.
  - This indicates that the variability in CPEPTID levels between `before`, `after`, and `after_2` is 14.83 times larger than the variability within subjects, suggesting a strong effect of time.
- **Interpretation**: A large F Value, combined with a small p-value (e.g., 0.0141 for CPEPTID), supports rejecting the null hypothesis, meaning at least one time point differs significantly from the others.

---

### **21. Num DF (Numerator Degrees of Freedom)**
- **Definition**: Num DF is the degrees of freedom associated with the **between-group variance** (i.e., the time factor in your repeated measures ANOVA). It reflects the number of independent pieces of information used to calculate the variance between time points.
- **Formula**:
$$ \text{Num DF} = k - 1 $$
  where \( k \) is the number of time points.
- **In Your Data**:
  - You have 3 time points: `before`, `after`, and `after_2`.
  - So, \( k = 3 \), and:
 $$ \text{Num DF} = 3 - 1 = 2 $$
  - For CPEPTID, Num DF = **2.0**, as shown in the output.
- **Purpose**: Num DF determines the shape of the F-distribution used to calculate the p-value. It reflects the number of independent comparisons among time points (e.g., you’re comparing 3 time points, so there are 2 independent differences).
- **Interpretation**: A Num DF of 2 is expected for 3 time points and is used in conjunction with Den DF to assess the significance of the F Value.

---

### **22. Den DF (Denominator Degrees of Freedom)**
- **Definition**: Den DF is the degrees of freedom associated with the **within-group variance** (i.e., the error term in repeated measures ANOVA). It reflects the amount of independent information available to estimate the random error and individual variability across subjects and time points.
- **Formula**:
  In repeated measures ANOVA, Den DF is calculated as:

$$
\text{Den DF} = (n - 1) \times (k - 1)
$$
  where:
  - \( n \) = number of subjects.
  - \( k \) = number of time points.
- **In Your Data**:
  - You have 3 subjects (\( n = 3 \)) and 3 time points (\( k = 3 \)).
  - So:
  $$ \text{Den DF} = (3 - 1) \times (3 - 1) = 2 \times 2 = 4 $$
  - For CPEPTID, Den DF = **4.0**, as shown in the output.
- **Purpose**: Den DF quantifies the degrees of freedom for the error term, which measures variability not explained by the time effect. It’s used with Num DF to determine the critical value of the F-distribution for significance testing.
- **Interpretation**:
  - A Den DF of 4 is low, reflecting the small sample size (3 subjects). Low Den DF reduces statistical power, making it harder to detect significant differences, even if they exist.
  - In repeated measures ANOVA, Den DF is smaller than in independent ANOVA because it accounts for the correlation between repeated measures on the same subjects.

---






