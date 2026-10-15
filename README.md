# Estimation & Bootstrapping — Quantifying Uncertainty Without a Formula

## Objective
I measured how uncertain an estimate is, first with the textbook formula and then with the bootstrap for cases where no formula exists.

## Methodology
- Loaded the UCI Adult Census Income data (32,561 respondents) and treated the full file as a population to draw samples from.
- Separated the standard deviation of the data from the standard error of an estimate, and checked the s/sqrt(n) relationship across four sample sizes.
- Built a parametric confidence interval for mean age from the CLT, and compared how fast the sampling distribution of the mean becomes normal for age and for capital gains.
- Ran the bootstrap by hand: resampled the data with replacement, recomputed the statistic on each resample, and read the interval from the percentiles of the results.
- Repeated the bootstrap with scipy.stats.bootstrap and compared percentile and BCa intervals on heavily skewed capital gains.
- Bootstrapped a two-sample difference in mean weekly hours between men and women.
- Asked an AI to write a bootstrap-interval function, revised my prompt once, and checked the function against my own intervals.
- Ran a coverage simulation on lognormal data at n=50 to check how often each 95% interval actually contains the true value.

## Key Findings
- **Standard deviation vs. standard error:** The standard deviation describes how spread out the data are and does not change with sample size. The standard error describes how precise the estimate is and shrinks with sqrt(n).
- **CLT speed:** The sampling distribution of the mean became normal quickly for age, but slowly for capital gains, which are heavily skewed.
- **A degenerate bootstrap:** The bootstrap SE of the median of 32,561 whole-number ages was exactly 0.0000. Every resample returned the same median, because with that many ties the median is pinned. The bootstrap was reporting a real property of the data, not failing.
- **Checking AI code:** I checked the AI's bootstrap function against my own intervals before trusting it, and revised my prompt once to decide how the function handles randomness.
- **Coverage of the mean:** The nominal 95% parametric interval covered 92.5% and the percentile bootstrap 91.5%. The gap is inside the Monte Carlo standard error of about 1.3 points, so the two methods are indistinguishable here, and both under-cover slightly.
- **Coverage of the median:** The mean-centred parametric interval covered only about 66.8% of the time, while the bootstrap held near its nominal level. When there is no formula for a statistic, the bootstrap gives an interval that actually targets it.
