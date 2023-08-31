## General t-test Background
### $t$-distribution: What is it?
Here are some facts about samples from normal populations will prove useful
1. The sample mean from a normal sample is itself normally distributed
2. Sample variance from a normal sample has a scaled $\chi^2$ distributed
3. Sample mean and sample variance from a normal sample are statistically independent
4. If $Z ~ N(0,1)$ is independent of a $\chi^2_d$ random variable, the quantity:
$$
\frac{Z}{\sqrt{\chi^2_d/d}}\sim t_d\,,
$$
a t-distribution with $d$ degrees of freedom

### The $t$-statistic
The $t$ statistic is the ratio of the difference of the sample mean and the value claimed from its hypothesized value to its standard error. 

Given a population mean $\mu$, the sample mean and variance $\bar{X}$ and $S^2$, the ratio:
$$
\frac{\bar X-\mu}{S/\sqrt{n}} = \frac{\sqrt n(\bar X-\mu)/\sigma}{S/\sigma}\sim t_{n-1}\,.
$$

## One Sample $t$-test
### Hypothesis Testing Workflow
1. Hypothesis
- The statement against what you are searching for evidence is called the null hypothesis and ide denoted as $H_0$
- The statement you claim is called the alternative hypothesis, $H_1$
$$
H_0\colon\ \theta = \theta_0 \quad \text{ vs }\quad \left\{
\begin{align}
& H_1\colon\ \theta > \theta_0 \quad \textrm{(upper-side alternative)}  \\
& H_1\colon\ \theta < \theta_0 \quad \textrm{(lower-side alternative)} \\
& H_1\colon\ \theta \ne \theta_0 \quad \textrm{(two-sided alternative)}
\end{align}
\right.
$$
2. Assumptions
- Each test will have to consider its own assumptions but a general assumption to be made is for each observation $X_1, X_2, ..., X_n$ is chosen at random from a population
	- Such random variables are *iid* (independently and identically distributed)
3. Test Statistic
- We use a test statistic to test if the data are consistent with $H_0$ such that the distribution of the test statistic is known assuming $H_0$ is true
	- Create a null distribution and use it to test our observed test statistic
$$
\frac{\bar X-\mu}{S/\sqrt{n}} = \frac{\sqrt n(\bar X-\mu)/\sigma}{S/\sigma}\sim t_{n-1}\,.
$$
- The **observed test statistic**, $t_0$, is where we plug our observed data into the formula for the test statistic
- A large observed test statistic values are taken as evidence of poor agreement with $H_0$
4. Significance
- The p-value is defined as the probability of getting a test statistic, $T$, as or more extreme than the value we observed, $t_0$, assuming that $H_0$ is true
	- For $H_1: \theta > \theta_0$, p-value = $P(T \geq t_0)$
	- For $H_1: \theta < \theta_0$, p-value = $P(T \leq t_0)$
	- For $H_1: \theta \neq \theta_0$, p-value = $2P(T \geq t_0)$
		- $\theta$ - Specified Theta
		- $\theta_0$ - observed theta
5. Decision
- Smaller the p-value, the stronger the evidence against $H_0$ and in favor of $H_1$
- Larger the p-value, does not mean that there is evidence that $H_0$ is true
- The level of significance, $\alpha$, is the strength of evidence needed to reject $H_0$

### One Sample $t$-test
Suppose we have a sample $X_1, X_2, ..., X_n$ of size $n$ drawn from a normal population with an unknown variance $\sigma^2$. Let $x_1, x_2, ..., x_n$ be the observed values and want to test population mean $\mu$
#### Beer Example
Is the mean beer content less than 375ml claim on the label?
```
beers = c(374.8, 375.0, 375.3, 374.8, 374.4, 374.9)
mean(x) # 374.87
sd(x) # 0.294
```
1. Hypothesis
- $H_0 : \mu = 375$ vs $H_1: \mu < 375$
2. Assumptions
- $X_i$ are *iid* random variables and follow $N(\mu,\sigma^2)$
3. Test Statistic
$$
T = \dfrac{\bar{X} - \mu_0}{S /\sqrt{n}}
$$
$$
t_0 = \dfrac{374.87 - 375}{0.29 /\sqrt{6}} = -1.11
$$
4. p-value 
- $P(t_5 \leq -1.11) = 0.16$
5. Decision
- The p-value $\geq$ our significance level (0.05) which is evidence that the data is consistent with the null hypothesis $H_0$

```
t.test(x, mu = 375, alternative = "less") # Automatic
# manual
n = length(x)
t0 = (mean(x) - 375) / (sd(x) / sqrt(n))
pval = pt(t0, n-1)
``` 
## Two Sample $t$-test
There are times where we want to test if the population means of two samples are different and we are left with two possible scenarios
- Two independent samples
- Two related samples

1. Hypotheses
- $H_0 : \mu_x = \mu_y$ vs $H_1 : \mu_x > \mu_y$ or $H_1 : \mu_x < \mu_y$ or $H_1 : \mu_x \neq \mu_y$  
2. Assumptions
- $X_1, ..., X_n$ are iid $N(\mu_x,\sigma^2)$
- $Y_1,...,Y_n$ are iid $N(\mu_y, \sigma^2)$
3. Test Statistic
$$
T = \dfrac{{\bar x} - {\bar y}}{s_p \sqrt{\frac{1}{n_x} + \frac{1}{n_y}}}
$$
$$
s^2_p = \dfrac{(n_x-1) s_{x}^2 + (n_y-1) s_{y}^2}{n_x+n_y-2}
$$

4. P-value
- $P(t_{n_x+n_y-2}\ge t_0)$ or
- $P(t_{n_x+n_y-2}\leq t_0)$ or
- $2P(t_{n_x+n_y-2}\ge |t_0|)$
5. Decision
- If p-value is less than confidence level, then there is evidence against $H_0$
- If p-value is greater than confidence level, the data is consistent with $H_0$
### Blood Platelet Aggregation Example
Blood samples are taken from 

## Paired sample $t$-test
Bloods sample from 11 individuals **before and after** they smoked a cigarettes are used to measure aggregation of blood platelets
- Used to determine whether the mean difference between two sets of observations is zero
```
before = c(25, 25, 27, 44, 30, 67, 53, 
           53, 52, 60, 28)
after =  c(27, 29, 37, 36, 46, 82, 57, 
           80, 61, 59, 43)
df = data.frame(before, after,
  difference = after - before)
df

t.test(df$after, df$before, paired = TRUE)
t.test(df$difference)
```

- Let $X_i$ and $Y_i$ be the blood platelet aggregation levels for the $i^{th}$ person before and after smoking.