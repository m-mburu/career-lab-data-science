Simple Statistical Tests
================
2026-02-19

### T test

- T test is a statistical test used to determine if there is a
  significant difference between the means of two groups. It is commonly
  used when the sample size is small and the population standard
  deviation is unknown.  
- Unequal variance t test is a type of t test that is used when the
  variances of the two groups being compared are not equal. It is also
  known as Welch’s t test. This test does not assume that the variances
  of the two groups are equal, and it adjusts the degrees of freedom
  accordingly.

## T test

A t test is used to assess whether the **mean** of one group differs
from the **mean** of another group (or from a reference value). Before
using a t test, you should first check (i) whether your outcome is
**continuous and approximately symmetric** (a mean is a sensible
summary), (ii) whether observations are **independent** within and
between groups (unless the design is paired), (iii) whether there are
**extreme outliers** that could dominate the mean, and (iv) whether the
outcome is **roughly normal within each group**—especially when sample
sizes are small. If normality is doubtful, consider a transformation
(e.g., log) or a non-parametric alternative.

Which t test you fit is determined by the **study design** and the
**variance structure**:

- **Unpaired (two-sample) Student’s t test (equal variances)** is
  appropriate when you have **two independent groups** (e.g., treatment
  vs control), the outcome is continuous, and it is reasonable to assume
  **similar variances** in the two groups. This “equal variances”
  assumption is about the *population* variances; in practice you assess
  it using subject-matter knowledge, a simple comparison of sample
  standard deviations, and/or a formal check such as Levene’s or an F
  test (noting these tests themselves can be sensitive to
  non-normality).

$$
t = \frac{\bar{X}_1 - \bar{X}_2}{s_p \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}}
$$

where $s_p$ is the pooled standard deviation, calculated as:

$$
s_p = \sqrt{\frac{(n_1 - 1)s_1^2 + (n_2 - 1)s_2^2}{n_1 + n_2 - 2}}
$$

- **Welch’s t test (unequal variances)** is used for **two independent
  groups** when the group variances may differ. It does not assume equal
  variances and adjusts the degrees of freedom accordingly. In applied
  work, Welch’s test is often a safe default for independent-group
  comparisons because it remains reliable even when variances and sample
  sizes are unbalanced.

$$
t = \frac{\bar{X}_1 - \bar{X}_2}{\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}}
$$

where the degrees of freedom are calculated using the
Welch-Satterthwaite equation:

$$
df = \frac{\left( \frac{s_1^2}{n_1} + \frac{s_2^2}{n_2} \right)^2}{\frac{\left( \frac{s_1^2}{n_1} \right)^2}{n_1 - 1} + \frac{\left( \frac{s_2^2}{n_2} \right)^2}{n_2 - 1}}
$$

- **Paired t test** is appropriate when the two measurements are
  **linked** (e.g., before/after measurements on the same person, or
  matched pairs). Here the analysis is done on the **within-pair
  differences**. The key assumption becomes that the **differences** are
  approximately normally distributed (not necessarily the raw
  measurements). If pairing exists in the design, you should not use an
  unpaired test because it ignores the correlation and loses efficiency.

$$
t = \frac{\bar{d}}{s_d / \sqrt{n}}
$$

where $\bar{d}$ is the mean of the differences, $s_d$ is the standard
deviation of the differences, and $n$ is the number of pairs.

A t test is **not appropriate** when the outcome is clearly
non-continuous (e.g., counts, binary outcomes), when the data are
heavily skewed with strong outliers and small sample sizes (making the
mean unstable), or when observations are not independent and you cannot
correctly model the dependence (e.g., clustered data without a
mixed/cluster-robust approach). In those settings, alternatives such as
the Wilcoxon rank-sum/signed-rank tests, permutation tests, or
regression models (linear, logistic, mixed models) may be more suitable.

#### Example of a two-sample t test in R

``` r
# Example of a two-sample t test in R
set.seed(123)
group1 <- rnorm(30, mean = 5, sd = 1)
group2 <- rnorm(30, mean = 6, sd = 1.5)
t.test(group1, group2, var.equal = TRUE) # Student's t test
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  group1 and group2
    ## t = -4.5254, df = 58, p-value = 3.041e-05
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -1.896105 -0.733118
    ## sample estimates:
    ## mean of x mean of y 
    ##  4.952896  6.267508

#### Example of Welch’s t test in R

``` r
# Example of Welch's t test in R
set.seed(123)
group1 <- rnorm(30, mean = 5, sd = 1)
group2 <- rnorm(30, mean = 6, sd = 1.5)
t.test(group1, group2, var.equal = FALSE) # Welch's t test
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  group1 and group2
    ## t = -4.5254, df = 54.849, p-value = 3.28e-05
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -1.8968167 -0.7324059
    ## sample estimates:
    ## mean of x mean of y 
    ##  4.952896  6.267508

#### Example of a paired t test in R

``` r
# Example of a paired t test in R
set.seed(123)
before <- rnorm(30, mean = 5, sd = 1)
after <- before + rnorm(30, mean = 1, sd = 0.5)
t.test(before, after, paired = TRUE) # Paired t test
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  before and after
    ## t = -14.287, df = 29, p-value = 1.172e-14
    ## alternative hypothesis: true mean difference is not equal to 0
    ## 95 percent confidence interval:
    ##  -1.2450901 -0.9332482
    ## sample estimates:
    ## mean difference 
    ##       -1.089169
