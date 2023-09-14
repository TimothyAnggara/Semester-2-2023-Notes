## What is Anova?
### Plant Growth
Results from an experiment to compare yields obtained under a control and two different treatment conditions were done on plant growth
```
# the data is already "in" R
data("PlantGrowth") 
library(tidyverse)
PlantGrowth |> ggplot() +
  aes(y = weight, x = group, 
      colour = group) + 
  geom_boxplot(coef = 10) + 
  geom_jitter(width = 0.1, size = 5) + 
  theme(legend.position = "none") +
  labs(y = "Weight (g)", x = "Group")
```
![[Pasted image 20230913222320.png]]
What should we do if we want to compare the means of the three different groups?
- Control, treatment 1, and treatment 2
- Does the mean of the control, treatment 1 and treatment 2 have the same mean or differ in some way?

### What does ANOVA stand for?
ANOVA is an abbreviation of the term "Analysis of Variance" and the term "variance" and the ANOVA procedure is mainly due to Fisher from the 1920 in a particular book he wrote
#### What is "Analysis of Variance?"
In its "simplest" form, Analysis of Variance is a generalization of a two sided two-sample $t$-test to 3 or more samples
- But. which two-sample $t$-test?

## $t$-test Revision
There are at least 3 different procedures which might be referred to as a "two-sample $t$-test" 
- Paired (two-sample) $t$-test
- Welch test (unequal variance two-independent sample $t$-test)
- Classical or Pooled two-independent-sample $t$-test
All the $t$-test described in above have a generic form and only differ in how the standard error is computed
$$
\frac{\bar{X}-\bar{Y}}{\text{SE}(\bar{X}-\bar{Y})}\,.
$$
### Paired (two-sample) $t$-test
For a paired two-sample $t$-test, it is assumed that the differences are identically and independently distributed normally with variance $\sigma^2_D$
- Under these conditions, the mean of the difference $\bar{D} = \bar{X} - \bar{Y}$ is normal with variance $\frac{\sigma_D^{2}}{n}$
- $\sigma_D^{2}$ is estimated using $S_D^{2}$, *the sample variance of differences*, giving a standard error of:
$$
\text{SE}(\bar{X}-\bar{Y})=\frac{S_D}{\sqrt{n}}\,.
$$
- Note that the test statistic is exactly distributed as $t_{n-1}$ under $H_0$
- Note that this is just a one-sample $t$-test applied to the differences
#### Sleep Data
We can try and two

### Welch (unequal variance two-independent-sample) $t$-test
In a Welch test, it is only assumed that each sample is normal, with different variances and different means, and all random variables are independent
- Under these conditions, the differences, $\bar{D} = \bar{X} - \bar{Y}$ is normal with variance
$$
\frac{\sigma^2_X}{m}+\frac{\sigma^2_Y}{n}\,.
$$
and its standard error is obtained by plugging the sample variances into this formula:
$$
\text{SE}(\bar{X}-\bar{Y})=\sqrt{\frac{S_X^2}{m}+\frac{S_Y^2}{n}}\,.
$$
- The test statistic is approximately, $t_v$ under $H_0$, where $v$ is some know function of ($m,n.\sigma_X \sigma_Y$)

### Classical two-(independent)-sample (equal variance) $t$-test
The "classic" two sample $t$-test which assumes the same assumptions as the Welch test with the extra assumption that the two populations variances are equal
- Under these conditions, the differences, $\bar{D} = \bar{X} - \bar{Y}$ is normal with a variance $\sigma^2 = (\frac{1}{m}+\frac{1}{n})$ 
- $\sigma^2$ is estimated using the pooled estimator:
$$
S_p^2= \frac{(m-1)S_X^2+(n-1)S_Y^2}{m+n-2}
$$
which we can then use to find the standard error by:
$$
\text{SE}(\bar{X}-\bar{Y})=S_p \sqrt{\frac{1}{m}+\frac{1}{n}}\,.
$$
- Note that the test statistic is exactly distributed as $t_{m+n-2}$ under $H_0$


#### Equal Variance Assumption
The assumption of the equal variance is quite crucial for the balidity of the Classical test because without strange things can happen if:
- Population variances are different
- Sample sizes are very different
Here is an example of what might happen
```
B = 10000
pval.Classical = pval.Welch = vector(length = B)
set.seed(123)
for(i in 1:B){
  x = rnorm(100, sd = 1)          # both samples have the same mean
  y = rnorm(20, sd = 3)           # smaller sample has bigger variance      
  pval.Welch[i] = t.test(x, y)$p.val
  pval.Classical[i] = t.test(x, y, var.equal = TRUE)$p.val
}
mean(pval.Welch < .05)        # Rejects about 5% of the time.
# 0.0487
mean(pval.Classical < .05)    # Rejects far too often!!
# 0.2887
```
### Some Comments
Out of the 3 different two-sample t-test, the classical test is the one that requires the most assumptions
- One could almost "do away" with:
	- Using a Welch test instead
	- Paired test if sample sizes are equal
		- Note that the differences are still iid normal and that it suffers a minor loss of power but is robust against positive correlation
Know what in ANOVA, we can only generalizing the classical test hence we must always be aware of the assumptions:
- Independence between samples
- Equal variance

## General ANOVA Decomposition
### ANOVA (In the case of $g$ groups)
1. Hypotheses: $H_0$: $\mu_1 = \mu_2 = ... \mu_g$ vs $H_1$ : at least one $\mu_i \neq \mu_j$
2. Assumptions: 
	- Observations are independent within each of the $g$ samples. 
	- Each of the $g$ populations have the same variances
		- $\sigma^2_1 = \sigma^2_2 = ... = \sigma^2_g = \sigma^2$
	- Each of the $g$ populations are normally distributed
		- You can rely on CLT if sample size is large enough
3. Test Statistic: $T = \frac{\text{Treatment Mean Sq}}{\text{Residual Mean Sq}}$. Under $T \sim F_{g-1,\ N-g}$ where $g$ is the number of groups
4. Observed Test Statistic: $t_0= \frac{\text{Observed Treatment Mean Sq}}{\text{Observed Residual Mean Sq}}$
5. P-value $P(T \geq t_0) = P(F_{g-1,\ N-g} \geq t_0)$. 
	- Note that we are always looking in the upper tail
6. Decision:
	- If the p-value is less than $\alpha$,  we reject the null hypothesis and conclude that the population mean of at least one group is significantly different to the others
	- If the p-value is greater than $\alpha$ we do not reject the null hypothesis and conclude that there is no significant difference between the population means

### Double Subscript Notation
Lets introduce some new notation so that we don't get confused when trying to specify specific entries in our groups.

We shall denote the data with two subscripts:
- $i$ to index the samples ("groups")
- $j$ to index the observations within samples/groups

Samples may have possibly different sizes across groups and let us denote $g$ as the number of groups
- We can let $y_{ij}$ to denote the observation on individual $j$ in the sample for group $i$
	- $j = 1,2,...,n_i$ and $i = 1,2,...,g$

### Normal model
We model $y_{ij}$ as the value taken by a random variable:
$$
Y_{ij}\sim \mathcal{N}(\mu_i,\sigma^2)\,,
$$
Basically what are saying is that our observations follow a normal distribution with some mean that may be group-specific but has a common variance

### Dot Notation Recall
- Recall that the dot notation basically means adding over that/those subscripts
- Total for sample $i$ is $\sum_{j=1}^{n_i}y_{ij}=y_{i\bullet}$
- average for sample  $i$ is $\dfrac{1}{n_i}\sum_{j=1}^{n_i}y_{ij}=\bar y_{i\bullet}$
- grand total of all observations is $\sum_{i=1}^g\sum_{j=1}^{n_i}y_{ij} = y_{\bullet\bullet}$
- overall average of all observations is $\dfrac{1}{N}\sum_{i=1}^g\sum_{j=1}^{n_i}y_{ij} = \bar y_{\bullet\bullet}$
- here $N = n_1 + ... + n_g$ is the total number of observations.

### General ANOVA Decomposition
The "weighted average" decomposition introduced for the two-sample $t$-test is a special case of a more general decomposition

The formula below is basically summing over each observation in each group the differences between the observation and the overall average of all observations squared: This is so called the total sum of squares
$$
\sum_{i=1}^g\sum_{j=1}^{n_i} (y_{ij}- \bar y_{\bullet\bullet})^2
$$The total sum of squares can be thought as a way of estimated the combined sample variance
- If we take our total sum of squares (Total SS) and divide by ($N-1$) and under the assumptions that our populations have the same mean and same variance, it can be thought of as an estimate of the overall variance
$$
\hat{\sigma}^2_0 = \frac{\text{Total SS}}{N-1} = \frac{\sum_{i=1}^g\sum_{j=1}^{n_i} (y_{ij}- \bar y_{\bullet\bullet})^2}{N-1}
$$
#### Decomposition
$$
\small{
\begin{align*}
\sum_{i=1}^g\sum_{j=1}^{n_i} (y_{ij}- \bar y_{\bullet\bullet})^2
&=\sum_{i=1}^g\sum_{j=1}^{n_i}
\left[ (y_{ij}-\bar y_{i\bullet})+(\bar y_{i\bullet}- \bar y_{\bullet\bullet})\right]^2\\
&=\sum_{i=1}^g\sum_{j=1}^{n_i}
\left[ (y_{ij}-\bar y_{i\bullet})^2+2(y_{ij}-\bar y_{i\bullet})(\bar y_{i\bullet}- \bar y_{\bullet\bullet}) + (\bar y_{i\bullet}-\bar y_{\bullet\bullet})^{2}\right]\\
&=\sum_{i=1}^g\sum_{j=1}^{n_i}
(y_{ij}-\bar y_{i\bullet})^2+
2
\sum_{i=1}^g
(\bar y_{i\bullet}- \bar y_{\bullet\bullet})
\underbrace{\sum_{j=1}^{n_i}
(y_{ij}-\bar y_{i\bullet})}_{=0}
+
\sum_{i=1}^g (\bar y_{i\bullet}-\bar y_{\bullet\bullet})^{2} \underbrace{\sum_{j=1}^{n_i}1}_{=n_i}\\
&=\underbrace{\sum_{i=1}^g\underbrace{\sum_{j=1}^{n_i} (y_{ij}-\bar y_{i\bullet})^2}_{=(n_i-1)s_i^2}}_{\text{sample variances}}
+
\underbrace{\sum_{i=1}^g n_i (\bar y_{i\bullet}-\bar y_{\bullet\bullet})^{2}}_{\text{sample means}} \\
& = \text{Residual SS} + \text{Treatment SS}
\end{align*}
}
$$
The details aren't really important but note that we are decomposing the total sum of squares into something that depends upon the individual group variances and something else that depends on the variation between individual group means and the overall grand mean

### Residual Sum of Squares; Residual Mean Square
The residual sum of squares of the differences between our observations and our the individual group means will follow a scaled chi square distribution 
$$
\sum_{i=1}^g\sum_{j=1}^{n_i}(Y_{ij}-\bar Y_{i\bullet})^2 = \sum_{i=1}^g\underbrace{(n_i-1)S_i^2}_{\sim\sigma^2\chi^2_{n_i-1}}\sim \sigma^2\chi^2_{N-g}
$$
If we take that residual sum of squares and divide it by $(N-g)$ that will give us this alternative estimator of the variance
- The common variance in our data as $\hat{\sigma}^2$
$$
\hat{\sigma}^2 = \frac{\sum_{i=1}^g\sum_{j=1}^{n_i}(Y_{ij}-\bar Y_{i\bullet})^2}{N-g}\sim \left( \frac{\sigma^{2}}{N-g}\right)\chi^2_{N-g}\,.
$$

### Treatment Sum of Squares
Under our decomposition we have our total sum of squares = the residual sum of squares + treatment sum of squares
$$
\small{
\begin{align*}
\underbrace{\sum_{i=1}^g\sum_{j=1}^{n_i} (Y_{ij}- \bar Y_{\bullet\bullet})^2}_{\sim \sigma^2\chi^2_{N-1}\ \text{ under }H_0} = \underbrace{\sum_{i=1}^g\sum_{j=1}^{n_i}(Y_{ij}-\bar Y_{i\bullet})^2}_{\sim \sigma^2\chi^2_{N-g}\ \text{ always}} + \underbrace{\color{}{\sum_{i=1}^g n_i (\bar Y_{i\bullet}-\bar Y_{\bullet\bullet})^{2}}}_{\sim\ ????}
\end{align*}
}
$$
- This treatment sum of square must follow a scaled chi-square distribution of $\sigma^2\chi^2_{g-1}$ so that it adds up to the degrees of freedom of the total sum of squares when the $H_0$ is true
- When the $H_0$ is not true, the distribution will tend to get bigger
	- One of the means will be further away from what we normally expect
- The ratio$\dfrac{\color{}{\sum_{i=1}^g n_i (\bar Y_{i\bullet}-\bar Y_{\bullet\bullet})^2}}{g-1}$ is known as the **Treatment Mean Square**

#### Treatment?
- The Treatment Sum of Squares is the generalization of the term $\left(\frac{\bar X-\bar Y}{\sqrt{\frac{1}{m}+\frac{1}{n}}}\right)^2$ in the analysis of the two-combined-sample variance
- It measures the variability of the same means in a certain sense 
$$
\text{Treatment Mean Square} = \frac{\text{Treatment SS}}{g-1} = \dfrac{\sum_{i=1}^g n_i (\bar Y_{i\bullet}-\bar Y_{\bullet\bullet})^2}{g-1}
$$
