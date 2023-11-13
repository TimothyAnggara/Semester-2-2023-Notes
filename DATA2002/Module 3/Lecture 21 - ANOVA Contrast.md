## Contrast
### Plant Growth
Results from an experiment to compare yields obtained under a control and two different treatment conditions
- We want to compare the means of the three different groups
```
plant_anova = aov(weight ~ group, data = PlantGrowth)
summary(plant_anova)
```
![[Pasted image 20230915202104.png]]
The p-value is less than 0.05 so we reject the null hypothesis and there is evidence to suggest at least one of the groups has a significantly different mean yield to the others.
- But how do we know which means are different?
	- ctrl vs trt1?
	- ctrl vs trt2?
	- ...
### Beyond ANOVA: contrasts
Recall that our model: for $i = 1,2,...,g$ and $j=1,2,...,n_i$ with the $j$-th observation from the $i$-th group in the sample is modelled as the value taken from:
$$
Y_{ij}\sim \mathcal{N}(\mu_i,\sigma^2)\,,
$$
- The above basically means that the observation that belongs to the $j$-th observation from the $i$-th group follows a normal distribution with mean equal to the population mean from the $i$-th group but every group follows the same variation
	- And all random variables are assumed independent
We can rewrite the formula above as:
$$
Y_{ij} = \mu_{i} + \varepsilon_{ij},
$$
where $\varepsilon_{ij}\sim \mathcal{N}(0,\sigma^2)$ is the error term.
### Contrast
- A **contrast** is a **linear combination** where the coefficients **add to zero**
	- In an ANOVA context, a contrast is a linear combination of **means**
- There are two kinds of contrast
	- Population contrast - contrasts involving the population group means, $\mu_i$'s'
	- Sample contrast - contrast involving the sample group means, $\bar y_{i\bullet}$'s and $\bar{Y_{i\bullet}}$'s 
### Distribution of sample contrasts
- Any $c_1,...,c_g$ with $c_{\bullet}=\sum_{i=1}^gc_i=0$ defines a sample contrast, $\sum_{i=1}^gc_i\bar Y_{i\bullet}$ 
- Under our normal-with-equal-variances model, this random variable has a distribution given by
$$
\sum_{i=1}^gc_i\bar Y_{i\bullet}\sim \mathcal{N}\left( \sum_{i=1}^gc_i\mu_i,\ \sigma^2 \sum_{i=1}^g \frac{c_i^2}{n_i} \right)\,.
$$
- The formula above is the corresponding population contrast and is the expected value of the (random) sample contrast
- Conversely, the observed sample contrast $\displaystyle\sum_{i=1}^gc_i \bar y_{i\bullet}$ is an *estimate* of the population contrast
### Behavior of contrasts under the null hypothesis
- Under the ANOVA null hypothesis: $H_0: \mu_1 = ... = \mu_g$
- which would mean that for the contrasts, all population contrasts are zero
$$
\sum_{i=1}^gc_i\mu_i=\sum_{i=1}^gc_i\mu=\mu\sum_{i=1}^gc_i=0\,;
$$
- Thus, all random sample contrasts have expectation zero:
$$
E \left( \sum_{i=1}^gc_i\bar Y_{i\bullet}\right)=\sum_{i=1}^gc_i\mu_i=0\,.
$$
- Hence, under the ANOVA null hypothesis, it may be rephrased as "all population contrasts are zero"
#### Maybe not what we want
- In some examples, the "ANOVA null hypothesis" may be "too strong"
	- We may only wish to test one (or more) "special" population contrasts are zero
- Also the "ANOVA null hypothesis" may not be rejected for the reason we want
	- Some contrasts may be non-zero, but are they the ones we are interested in?
#### $t$-test for individual contrasts
- Suppose we really only want to test that $H_0\colon\ \sum_{i=1}^gc_i\mu_i=0$ for some "special contrasts" given by $c_i, ..., c_g$
- We can perform the ANOVA Mean-Square Ratio $F$-test, but can we do better?
	- We can perform a more "targeted" $t$-test using the corresponding sample contrast and the **residual mean square**
- The corresponding (random) sample contrast
$$
\sum_{i=1}^gc_i\bar Y_{i\bullet}\sim \mathcal{N} \left(\sum_{i=1}^gc_i\mu_{i}, \sigma^2\sum_{i=1}^g \frac{c_i^2}{n_i}\right)\,.
$$
- The standardized version
$$
\frac{\sum_{i=1}^gc_i\bar Y_{i\bullet}-\sum_{i=1}^gc_i\mu_i}{\sigma\sqrt{\sum_{i=1}^g \frac{c_i^2}{n_i}}} \sim \mathcal{N}(0,1).
$$
- replacing $\sigma$ with the denominator with $\hat{\sigma}=\sqrt{\text{Residual MS}}\sim \sqrt{\chi^2_{N-g}/(N-g)}$ which gives
$$
\frac{\sum_{i=1}^gc_i\bar Y_{i\bullet}-\sum_{i=1}^gc_i\mu_i}{\hat\sigma\sqrt{\sum_{i=1}^g \frac{c_i^2}{n_i}}} \sim t_{N-g}\,.
$$
- Thus a $t$statistic for testing the hypothesis that $\sum_{i=1}^{g}c_i\mu_i=0$ is:
$$
\frac{\sum_{i=1}^gc_i\bar Y_{i\bullet}}{\hat\sigma\sqrt{\sum_{i=1}^g \frac{c_i^2}{n_i}}}
$$
	- which has a $t_{N-g}$ distribution if $\sum_{i=1}^{g}c_i\mu_i=0$
- This generalizes the two-sample $t$-statistic

## Yield
Let $\mu_1, \mu_2, \mu_3$ represent the population means of treatment 1, treatment 2, and the control group
- Consider if there is a different between threatment 1 and treatment 2 this corresponds the contrast coefficients, $c_1 = 1, c_2 = -1, c_3 = 0$
```
plant_summary = PlantGrowth |> 
  mutate(group = factor(group, levels = c("trt1","trt2", "ctrl"))) |> 
  group_by(group) |> 
  summarise(n = n(), mean_weight = mean(weight)) |> 
  mutate(contrast_coefficients = c(1, -1, 0),
         c_ybar = mean_weight * contrast_coefficients)
plant_summary
```
![[Pasted image 20230915211653.png]]
```
sum(plant_summary$c_ybar) # sample contrast
# -0.865
```
#### Residual Standard Error
- Recall from the ANOVA analysis from earlier
```
plant_anova = aov(weight ~ group, data = PlantGrowth)
summary(plant_anova)
```
![[Pasted image 20230915202104.png]]
- The residual mean square is the estimate of $\sigma^2$, the population variance within each group
- The residual standard error is $\sqrt{\text{Residual Mean Sq}}$, the estimate of $\sigma$, the population standard deviation within each group
- To extract the terms we need for calculating the test statistic, we will use the `tidy()` function from the **broom** package
```
library(broom)

resid_ms = tidy(plant_anova)$meansq[2]
resid_ms # 0.3886

resid_se = sqrt(resid_ms)
resid_se # 0.6233746
```
#### Calculating the Test Statistic
- The observed test statistic for the contrast is 
$$
t_0 = \frac{\sum_{i=1}^gc_i\bar y_{i\bullet}}{\hat\sigma\sqrt{\sum_{i=1}^g \frac{c_i^2}{n_i}}} = \frac{\bar{y}_1 - \bar{y}_2 }{\sqrt{\text{Residual Mean Sq}}\times \sqrt{\frac{1^2}{10} + \frac{(-1)^2}{10} + \frac{0}{10}}}
$$
```
(n_i = plant_summary |> pull(n))
(ybar_i = plant_summary |> pull(mean_weight))
(c_i =  plant_summary |> pull(contrast_coefficients))
(se = sqrt(resid_ms * sum((c_i^2) / n_i)))
(t_stat = sum(c_i * ybar_i)/se)
# observed test statistic
(t_stat = sum(c_i * ybar_i)/se)
```
#### Calculating the p-value
When calculating the p-value, note that the test statistic has a $t_{N-g}$ distribution if  $\sum_{i=1}^gc_i\mu_i=0$ which is the same as the degrees of freedom as the denominator degrees of freedom from the ANOVA
```
# observed test statistic
(t_stat = sum(c_i * ybar_i)/se) # -3.1028

plant_anova$df.res # 27

2*pt(abs(t_stat), df = plant_anova$df.res, lower.tail = FALSE) # 0.0045
```
- Note that this is better than an ordinary two-sample $t$-test because it can have a (potentially) smaller standard error hence a better estimate of $\sigma$
## Confidence Intervals
A two-sided 95% confidence interval for a "special contrast" considered above is given as follows:
```
# The multipler t_star is determined by the formula below
t_star = qt(0.975, df = 27)

# Formula to obtain the interval
sum(c_i * ybar_i) + c(-1,1) * t_star * se
```
From the confidence interval above, we would reject the null hypothesis because 0 is not in the confidence interval