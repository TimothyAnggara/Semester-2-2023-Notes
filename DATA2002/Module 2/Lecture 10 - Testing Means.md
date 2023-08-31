## Learning Objectives:
- General t-test background
- One sample t-test
- Two-sample t-test
- Paired samples t-test

## General t-test background

### What is the t-distribution
Here are some facts about samples from normal population what will be useful:
1. Sample mean from a normal sample is itself normally distributed
	- If you draw a sample from a normally distributed population calculate the sample's mean and do it repeatedly for many samples, the distribution of all those means will be normal because of the CLT
1. Sample variance from a normal sample has a scaled $\chi^2$ distribution
	- If you calculate the sample variance repeatedly from many samples drawn from a normal distribution, the distribution of those sample variances will follow a chi-squared distribution
1. Sample mean and sample variance from a normal sample are statistically independent
	- The calcualted sample mean and sample variance of a normally distributed sample does not affect each other
	- Knowing the sample mean doesn't give any information about what the variance will be and vice versa
1. If $Z ~ N(0,1)$ is independent of a $\chi^2_d$ random variable, the quantity with a t-distribution with d degrees of freedom:
$$
\frac{Z}{\sqrt{\chi^2_d/d}}\sim t_d\,,
$$
	- The equation above explains how the t-distribution is derived, if you take a value Z, from a standard normal distribution and divide it by the square root of a value from a chi-squared distribution, the resulting value follows a t-distribution
	- The shape of a t-distribution depends on its degrees of freedom
		- The larger the degrees of freedom, the more it will look like a normal distribution
### The t-statistic
Given a population mean $\mu$, the sample mean and variance $\bar{X}$ and $S^2$, the ratio 
$$
\frac{\bar X-\mu}{S/\sqrt{n}} = \frac{\sqrt n(\bar X-\mu)/\sigma}{S/\sigma}\sim t_{n-1}\,.
$$
- The numerator $N(0,1)$
- The denominator is $\sqrt{\chi^2_{n-1}/(n-1)}$, independently of the numerator
In many statistical application, we have a model where a certain statistic has this general form:
- Some estimator of some parameter is normally distributed
- Standard error based on the data has a distribution like $sqrt(\chi^2_d / d)$ times the true SD of the estimator (for some $d$) and is independent of the estimator;
- Thus the ratio $\frac{estimator - true \ value}{standard error} ~ t_d$
## One sample t-test
Asking, is the sample mean equal to some value

### Beer contents
Beer content in a pack of six bottles (in milliliters) are:
```
y = c(374.8, 375.0, 375.3, 374.8, 374.4, 374.9)
```
Is the mean beer content less than the 375 ml claimed on the label?
```
library("ggplot2")
df = data.frame(y)
set.seed(124)
p1 = ggplot(df, aes(x = "", y = y)) +
  geom_boxplot(alpha = 0.5, coef = 10) + 
  geom_dotplot(binaxis = 'y', 
               stackdir = 'center') + 
  geom_hline(yintercept = 375, 
             colour = "blue",
             linetype = "dashed") + 
  labs(y = "Beer volume (ml)", x = "") +
  theme_bw(base_size = 24) + 
  theme(axis.ticks.x = element_blank(),
        axis.text.x = element_blank())
```
![[Pasted image 20230821162105.png]]

### Hypothesis Testing
- **Hypotheses**: $H_0: \theta = \theta_0 \ vs \ H_1: \theta > \theta_0 \ or \ \theta < \theta_0 \ or \ \theta \neq \theta_0$
- **Assumptions**: Our samples follow some distribution
- Test statistic: $T = f(X_1, X_2, ..., X_n)$
- observed test statistic: $T = f(x_1, x_2, ..., x_n)$
- Significance: p-value = $P(T \geq t_0) \ or \ P(T \leq t_0) \ or \ 2P(T \geq |t_0|)$
- Decision: If the p-value is less than $\alpha$, there is evidence against $H_0$
### Hypothesis
- Statement against which you search for evidence is called the null hypothesis and ide denoted by $H_0$
	- The "No Difference" Statement
- Statement you claim is the alternative hypothesis and is denoted by $H_1$
$$
H_0\colon\ \theta = \theta_0 \quad \text{ vs }\quad \left\{
\begin{align}
& H_1\colon\ \theta > \theta_0 \quad \textrm{(upper-side alternative)}  \\
& H_1\colon\ \theta < \theta_0 \quad \textrm{(lower-side alternative)} \\
& H_1\colon\ \theta \ne \theta_0 \quad \textrm{(two-sided alternative)}
\end{align}
\right.
$$
### Assumptions
Each observations $X_1, X_2, ..., X_n$ is chosen at random from a population and these variables are iid, (independently and identically distributed)

### Test Statistic
Since observations $X_i$ vary from sample to sample, we can never be sure whether $H_0$ is true or not, thus we can use a test statistic $T = f(X_1, ..., X_n)$ to test if the data are consistent with $H_0$ such that the distribution of $T$ is known assuming $H_0$ is true

The **observed test statistic**, $t_0$, is where we plug our observed data into the formula for the test statistic

### Significance
The p-value is defined as the probability of getting a test statistic, $T$, as or more extreme than the value we observed, $t_0$, assuming that $H_0$ is true
![[Pasted image 20230821192014.png]]
### Decision
Based of the p-value that we calculated,  we can either retain or reject the null hypothesis. 
- The smaller the p-value, the stronger the evidence against $H_0$ and in favor of $H_1$
- A larger p-value does not mean that there is evidence that $H_0$ is true
![[Pasted image 20230821192147.png]]
### Beer Content Example
```
x = c(374.8, 375.0, 375.3, 374.8, 374.4, 374.9)
mean_x = mean(x)
sd_x = sd(x)
n = length(x)
t0 = (mean_x - 375) / (sd_x / sqrt(n)))
pval = pt(t0, n-1) # 0.159 retain null hypothesis i.e. Beer content mean is 375 ml
```
## Two-sample t-test
There are times that we want to test if the population means of two samples are different and we are left with two possible scenarios
- Two independent samples
	- Blood samples taken from 11 smokers and 11 non-smokers
- Two related samples (dependent samples or repeated measures)
	- Before and after scenarios
	- Before medication and after medication
	- Twins taking a medication and not

### Smokers and Blood Platelet Aggregation
```
non_smokers = c(25, 25, 27, 44, 30, 67, 
                53, 53, 52, 60, 28)
smokers =  c(27, 29, 37, 36, 46, 82, 
             57, 80, 61, 59, 43)
dat = data.frame(
  platelets = c(non_smokers, smokers),
  status = c(rep("Non smokers", 
                 length(non_smokers)),
             rep("Smokers", 
                 length(smokers)))
)
library(dplyr)
sum = dat %>% 
  group_by(status) %>% 
  summarise(Mean = mean(platelets),
            SD = sd(platelets), 
            n = n())
            
```
![[Pasted image 20230821195713.png]]
```
library(ggplot2)
ggplot(dat) + aes(x = status, y = platelets) +
  geom_boxplot() + 
  geom_jitter(width = 0.15, size = 3, colour = "blue") + 
  labs(x = "", y = "Blood platelet\naggregation")
```
![[Pasted image 20230821200100.png]]
As we can see from the boxplots, most of the interquartile space is within each other so we can expect a somewhat large p-value
![[Pasted image 20230821200234.png]]
Let $X_i$ be the blood platelet aggregation levels for the $i^{th}$ non-smoker and $Y_j$ the levels for the $j^{th}$ smoker. Let $\mu_S$ and $\mu_N$ be the population mean platelet aggregation levels for smokers and non-smokers respectively
![[Pasted image 20230821201113.png]]
```
t.test(smokers, non_smokers, alternative = "two.sided", var.equal=TRUE)
```
### Equal Variance Assumption
In the assumptions for the two sample t-test, we assumed that the variance of the two samples are the same. If we have two samples and lets say that the assumption does not hold, we can use the **Welch Test**

### Welch Two Sample t-test
Welch developed an alternative test which does **not** assume equal population variances. In the Welch two sample t-test, the test statistic $T$:
$$
T = \dfrac{\bar X-\bar Y}{\sqrt{\frac{S_X^2}{n_x}+\frac{S_Y^2}{n_y}}}\,.
$$
![[Pasted image 20230825175855.png]]
### Welch Statistic not a proper t-statistic
The Welch test statistic, is not a usual $t$-statistic since the denominator is NOT a scaled $\chi^2$ independent of the numerator however the statistic still has an approximate $t$-distribution where the degrees of freedom is not estimated from data

In R, we can tell it to use a Welch statistic by placing a parameter `var.equal = FALSE` and R does this by default
```
t.test(smokers, non_smokers, alternative = "two.sided")
```
Note: The p-value is much the same as the equal-variance assumption variance assumptions


- 