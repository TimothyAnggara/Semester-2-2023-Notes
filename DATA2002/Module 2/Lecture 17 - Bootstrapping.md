## Speed of Light


## Confidence Intervals

### Estimation vs Hypothesis Testing
**Estimation**
- A population parameter is unknown
- Use sample statistics to generate estimates of the population parameter
**Hypothesis Testing**
- Explicit statement or hypothesis regarding the population parameter
- Test Statistics are generated which will either support or reject the null hypothesis
### Confidence Intervals
In a report we should avoid reporting just a point estimate for a sample, we should always include a measure of variability: 
- $\hat{\theta} \ \pm$ margin of error
- Where $\hat{\theta}$ is the point estimate (e.g. sample mean)

The margin of error usually takes the form
$$
Margin \ of \ error = critical \ value \times SE(\hat{\theta})
$$
- Critical value is some quantile from an appropriate distribution and SE($\theta$) is the standard error of a point estimate

Let the point estimate $\hat{\theta}$ of a parameter $\theta$ does not show its variability across samples, to show estimation precision, we should find an interval estimate
$$
P(\hat \theta_L\le \theta\le \hat\theta_R)=1-\alpha,
$$
- The random interval  ($\hat{\theta_L}$,$\hat{\theta_L}$) is valled a 100(1-$\alpha$)% confidence interval
- Note that the Confidence Interval can be only used for normal distribution?

### Confidence Intervals for the mean
### Meaning Of the Confidence Interval
- Suppose a 95% confidence interval for the mean $\mu$ is (a,b)
	- This **DOES NOT** mean that 95% of the means $\mu$ are in (a,b) that is$P(a < \mu <b)$ since $\mu$ is fixed but unknown parameter
	- This **DOES NOT** mean $P(a < $\bar{X} < b) = 0.95$ where $\bar{X}$ is the sample mean since CI is for the true mean $\mu$ not for the sample mean $|bar{X}$
	- It **DOES** mean that if we draw a large number of random samples and compute for teach sample's 95% CI, about 95% of these CIs will contain $\mu$
		- Can be described as a range of possible values for the population parameter

### Distributional Assumptions
- One of the assumptions to be able to reliably use the confidence interval is for the data to follow a normal distribution
- What should happen if this doesn't hold?
  1. Guess the distribution of the data and this the distribution to calculate critical values for confidence intervals (**RISKY**)
2. Use Bootstrap resampling to empirically model the distribution of the data

## Bootstrap
Bootstrapping is a computational process that allows us to make inferences about the population where no information is available about the population.
- The classic approach to bootstrapping to repeatedly resample from the sample with replacement

#### Speed of Light
Simon Newcomb measured the time required for light to travel from his laboratory to the Potomac river
- He performed 66 experiments and  these measurements were used to estimate the speed of light
```
mean(speed) # 26.21212

# start of Bootstrapping
set.seed(123)
B = 10000
result = vector("numeric", length = B)
for(i in 1:B){
  newData = sample(speed, replace = TRUE)
  result[i] = mean(newData)
}
```
To find the confidence intervals that we want we use the `quantile` function to find at what significance level of the confidence interval are we interested.
If we are interested in the 95% confidence interval then we would write
```
quantile(result, c(0.025, 0.0975))
```
- Note that the bootstrap confidence interval is not symmetric about the mean

## Final Remarks
Bootstrapping is useful when:
- the theoretical distribution of a statistic is complicated or unknown (e.g. coefficient of variation, quantile regression parameter estimates, etc.)
- the sample size is too small to make any sensible parametric inferences about the parameter
#### Advantages
- Bootstrapping frees us from making parametric assumptions to carry out inferences
- Provides answers to problems for which analytic solutions are impossible
- Can be used to verify, or check the stability of results
- Asymptotically consistent