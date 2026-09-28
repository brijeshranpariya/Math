# Probability Distributions on Transaction Data

Part B of the practical. I used a dataset of 220 transactions from January 2023 (`spread_locator_dataset - spread_locator_dataset.csv.csv`) to fit different probability distributions in Python with NumPy, Pandas, SciPy, Statsmodels, Matplotlib and Seaborn. All the work is in `Transaction_Distribution_Analysis.ipynb`.

## What I learned

### Bernoulli and Binomial

- Bernoulli is a single trial with two outcomes. Here, one transaction is either Success (1) or Fail (0).
- The only parameter is p. Mean = p and variance = p(1 − p), which is largest when p = 0.5.
- Binomial is the number of successes in n Bernoulli trials. I counted the successful transactions in each week, with n = the number of transactions that week.
- To check the fit, I compared each week's count with the 95% range from `binom.ppf`.

### Poisson

- Used for the number of events in a fixed time period, such as transactions per day.
- It has one parameter, λ, which is both the mean and the variance. Checking variance / mean is a quick way to test whether Poisson makes sense.
- For the chi-square goodness-of-fit test, I had to group small counts together so that no expected count was too small.

### Log-Normal and Power Law

- If log(X) is normal, then X is Log-Normal. It fits positive data with a long right tail, like money amounts.
- A Power Law (Pareto) has a much heavier tail. It needs a starting value xmin, and α can be estimated as 1 + n / Σ ln(x / xmin).
- A histogram on a log-log scale makes the difference between the two easy to see.
- To compare distributions I used the log-likelihood, AIC (lower is better) and the KS test.

### Q-Q plot

- A Q-Q plot compares the quantiles of the data with the quantiles of a normal distribution. If the data is normal, the points fall on a straight line.
- If the points curve upward at the right end, the data has a long right tail.
- Shapiro-Wilk gives a p-value for normality, and it agreed with the plot.

### Box-Cox transform

- Box-Cox finds the power λ that makes the data closest to normal. It only works on positive values.
- λ = 0 means a log transform and λ = 1 means no change. I got λ ≈ −0.18, which is close to a log.
- After the transform, the spread was almost the same in every region, so the variance was stabilised.

### Z-scores

- z = (x − mean) / std shows how many standard deviations a value is from the mean.
- A normal-based probability from z is only reliable if the data is actually normal. Here it overestimated P(amount > ₹5000).
- Values with |z| > 3 are good candidates for outliers.

### PDF and CDF

- The PDF shows where values are concentrated. The CDF gives P(X ≤ x) directly.
- Plotting the empirical CDF against a fitted CDF is an easy way to see how well a distribution fits.

## Results

| Variable                       | Distribution | Parameters                  | Fit                                |
| ------------------------------ | ------------ | --------------------------- | ---------------------------------- |
| Transaction status             | Bernoulli    | p = 0.445                   | –                                  |
| Weekly successful transactions | Binomial     | n = weekly total, p = 0.445 | All weeks inside the 95% range     |
| Transactions per day           | Poisson      | λ = 7.10                    | Chi-square p = 0.19 (not rejected) |
| Transaction amount             | Normal       | –                           | KS p < 0.001 (rejected)            |
| Transaction amount             | Log-Normal   | μ = 8.00, σ = 0.47          | KS p = 0.90, lowest AIC            |
| Transaction amount             | Power Law    | α = 1.76                    | KS p ≈ 0 (rejected)                |

P(amount > ₹5000):

| Method      | Probability |
| ----------- | ----------- |
| Normal (z)  | 0.205       |
| Log-Normal  | 0.138       |
| Actual data | 0.114       |

What I took away from the results:

- Log-Normal is the best fit for transaction amounts. The log Q-Q plot and Box-Cox both pointed to the same thing.
- The normal model was wrong for the amounts because of the long right tail and a few very large transactions.
- Less than half of the transactions succeeded, and the daily number of transactions was fairly steady at about 7.

## Running it

```
pip install numpy pandas scipy statsmodels matplotlib seaborn jupyter
jupyter notebook Transaction_Distribution_Analysis.ipynb
```
