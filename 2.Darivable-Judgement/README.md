# Hypothesis Testing on Health Records

Part B of the practical. I used a dataset of 1000 patient records (`health_records.csv`) to practise hypothesis testing in Python with Pandas, SciPy and Statsmodels. All the work is in `Health_Records_Hypothesis_Testing.ipynb`.

## What I learned

### Writing hypotheses

- H₀ always says "no effect" or "no difference". H₁ says there is one.
- You should fix the significance level (I used α = 0.05) before running the test, not after seeing the p-value.
- "Fail to reject H₀" is not the same as "H₀ is true". It only means there isn't enough evidence against it.

### Confidence intervals

- A CI is a range where the true population mean probably lies: mean ± t × (s / √n).
- A higher confidence level gives a wider interval. For age, going from 90% to 99% made the interval noticeably wider.
- A bigger sample gives a narrower interval, because the standard error gets smaller.
- If the CIs of two groups don't overlap, that's already a strong hint that the groups really are different.

### Critical value vs p-value

- There are two ways to make the same decision:
  - Reject H₀ if the test statistic goes past the critical value.
  - Reject H₀ if p < α.
- They always agree. I checked this in every test.
- Useful critical values at α = 0.05: z = ±1.96, t with df = 24 is ±2.064, chi-square with df = 2 is 5.99.

### z-test vs t-test

- Use a z-test when the samples are large (n > 30 in each group).
- Use a t-test when the sample is small or σ is unknown. The t-distribution has fatter tails, so it needs stronger evidence.
- With large samples, t and z are almost the same (t with df = 999 is about 1.962).
- Check equal variances with Levene's test. If the groups are very unbalanced, Welch's t-test is the safer choice.
- A small sample can miss a real difference. With 25 patients, the BMI test was not significant (p = 0.89). The same test on all 1000 patients was highly significant. This is what low power looks like.

### Chi-square test

- Used for two categorical variables. It compares the counts we observe with the counts we would expect if the variables were independent.
- df = (rows − 1) × (columns − 1), and every expected count should be at least 5.
- Cramér's V shows how strong the association is, which the p-value alone doesn't tell you.

### ANOVA

- Used to compare means across more than two groups. Running many t-tests instead would increase the chance of a false positive.
- F = variance between groups / variance within groups.
- If a yes/no outcome is coded as 0/1, the group mean is simply the disease rate.
- ANOVA only says that _some_ group is different. A Tukey HSD test is needed to find out which pairs differ.

### Covariance and correlation

- Covariance shows the direction of a relationship, but its size depends on the units, so it's hard to compare.
- Correlation (r) is covariance scaled to between −1 and +1, so it shows strength as well.
- r² is the share of variation explained. For age vs BMI, r = 0.15 means only about 2% explained.
- With a large sample, even a very weak correlation becomes "significant". Always look at the effect size, not just the p-value.
- Correlation does not mean causation.

## Results

| Hypothesis                     | Test                | p-value | Decision                       |
| ------------------------------ | ------------------- | ------- | ------------------------------ |
| Smoking vs diabetes            | Chi-square          | 0.52    | Fail to reject H₀              |
| Smoking vs hypertension        | Chi-square          | 0.002   | Reject H₀                      |
| BP: smokers vs non-smokers     | z-test              | < 0.001 | Reject H₀                      |
| BMI: male vs female            | z-test              | 0.056   | Fail to reject H₀              |
| BMI: diabetic vs non-diabetic  | Welch t-test        | < 0.001 | Reject H₀                      |
| Age group vs diabetes rate     | ANOVA               | 0.001   | Reject H₀                      |
| Age group vs hypertension rate | ANOVA               | < 0.001 | Reject H₀                      |
| Age vs BMI                     | Pearson correlation | < 0.001 | Reject H₀ (but weak, r = 0.15) |

What I took away from the results:

- Age had the biggest effect overall, especially on blood pressure and hypertension.
- A higher BMI was clearly linked to diabetes.
- Smoking was linked to blood pressure and hypertension, but not to diabetes. This surprised me.

## Running it

```
pip install numpy pandas scipy statsmodels matplotlib seaborn jupyter
jupyter notebook Health_Records_Hypothesis_Testing.ipynb
```
