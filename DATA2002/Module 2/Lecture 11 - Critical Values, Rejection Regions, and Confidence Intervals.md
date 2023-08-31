## Learning Objectives
- Random Variables
- Critical values and confidence Intervals
- Rejection Regions
- Rejection region on the data scale

## Random Variables
### Random Variable Basics
A random variable can be thought of as a mathematical object which takes certain values with certain probabilities.
- This mathematical object can either be discrete or continuous 
- We can also "Approximate" a continuous random variable into a discrete one
	- The method for approximation, is placing continuous variables into "bins" of discrete values
		- We can place heights of people into 180-185 cm, 175-180, ... bins.

A simple discrete random variable $X$ can be described as a *single random* draw from a "box" containing tickets, each with numbers written on them
- $E(X) = \mu$
	- Average of the numbers in the box
- $Var(X) = \sigma^2$
	- The population variance of the numbers in the box
- $SD(X) = \sigma$
### Random Sample with Replacement
Next, consider taking random sample of size $n$ with replacement, denoting the values, $X_1, X_2, ... ,X_n$
- This means that one of all possible samples of size $n$ is chosen in such a way that each is equally likely
- If we have a box with 1,2,3 tickets and we draw twice, this means that we have (1,1), (1,2), (1,3), (2,1), (2,2), (2,3), (3,1), (3,2), (3,3) possible samples and each sample is **equally likely**.

If there are $N$ tickets in the box, how many such samples are there?
- If there are $N$ tickets and we are drawing $n$ samples, there will be $N^n$ possible samples that are equally likely
Note that, it turns out that these $X_i$'s are independent and identically distributed which means that:
- Each $X_i$ has the same distribution as a single draw
- The $X_i$'s are all mutually independent

Consider now taking the total $T = \sum^n_{i=1}{X_i}$
- What is E(T) and Var(T)?

#### Expectation of Sums of Random Variables
For E(T), The expectation of a sum is *always* the sum of the expectations,
- Recall if E(X) = $\mu
$$
\begin{align}
\operatorname{E}(T) & = \operatorname{E}\left(X_1+\cdots+X_n\right) \\
& = \operatorname{E}(X_1)+\cdots+\operatorname{E}(X_n) \\
& = \underbrace{\mu+\cdots+\mu}_{n\textrm{ terms}} \\
& = n\mu\,.
\end{align}
$$
And multiplying by a constant for any random variable $X$ and any constant C.
$$
E(cX) = cE(X)
$$
#### Variance of Sums of Random Variables
For Var(T), the variance of a sum is not always the sum of the variances, however it is if $X_i$'s are **Independent**. So,
$$
\begin{align}\operatorname{Var}(T) & = \operatorname{Var}(X_1+\cdots+X_n)  \\
& = \operatorname{Var}(X_1)+\cdots+\operatorname{Var}(X_n)  \qquad\textrm{(by independence)} \\
& = \underbrace{\sigma^{2}+\cdots+\sigma^{2}}_{n\text{ terms}} \\
& = n\sigma^2\,.
\end{align}
$$
And when multiplying with a constant, for any variable $X$ and any constant $c$,
$$
Var(cX) = c^2Var(X)
$$

#### Sample mean
Consider the sample mean  $\bar X=\dfrac{1}{n}\displaystyle\sum_{i=1}^{n}X_i = \frac{1}{n}T$
- The latex code above is basically summing up all the samples that we took and divided it by the number of samples

What is $E(\bar{X})$ and what is Var($\bar{X}$)
Since $\bar{X} = \frac{1}{n}T$, 
$$
\begin{align*}
\operatorname{E}(\bar X) &= \operatorname{E}\left(\frac{1}{n}T\right)=\frac{1}{n}\operatorname{E}(T)=\frac{1}{n}n\mu=\mu\,.\\            
\operatorname{Var}(\bar X)&=\operatorname{Var}\left(\frac{1}{n}T\right)=
\left(\frac{1}{n}\right)^2\operatorname{Var}(T)=\frac{1}{n^2}\,n\sigma^2=\frac{\sigma^2}{n}\,.
\end{align*}
$$
### Estimating $\mu$
We model data $x_1, ..., x_n$ as values taken by such a sample $X_1, ...,X_n$ and we are interested in "estimating" or "learning" $\mu$, which is an "unknown population mean"
- In this case, the *estimator* is the sample mean $\bar{X}$
- The *estimate* is $\bar x=\frac1n\sum_{i=1}^nx_i$, the observed value of the mean of the data
- I don't quite understand the estimator and estimate thing and how we are estimating mu

An important theoretical quantity is the standard error, the standard deviation of the estimator
$$
\operatorname{SE}=\operatorname{SD}(\bar X) = \sqrt{\operatorname{Var}(\bar X)}=\frac{\sigma}{\sqrt{n}}\,.
$$
- The standard error is (in general) also an unknown parameter


### Importance of Standard Error
- The standard error is important to know since it tells us the "likely size of the estimation error"
- An estimate on its own is not very useful but with how accurate or reliable the estimate is will be very useful
	- This is what the standard error provides
- Unfortunately, in most context, the standard error is unknown but we can usually (also) estimate the standard error

### Estimating the Standard Error
The standard error (at least when estimating a population mean $\mu$) involves the (usually unknown) population variance $\sigma^2$
$$
SE = \frac{\sigma}{\sqrt{n}}
$$
And fortunately, we can usually estimate $\sigma^2$ using the sample variance
$$
S^2= \frac{1}{n-1}\sum_{i=1}^n (X_i-\bar X)^2\,.
$$
With a corresponding estimated standard error:
$$
\widehat{\operatorname{SE}} = s/\sqrt{n}\,,
$$
Where $s^2=\frac{1}{n-1}\sum_{i=1}^n(x_i-\bar x)^2$ is the observed value of the sample variance

- What is $S^2$? Is it the population sample variance that we want to know and $s^2$ is the sample variance that we observed when we take our sample?


## Critical Values and Confidence Intervals
### More Precise Inference 
Usually, we want to know if a given value $\mu_0$ is a "plausible value" for the unknown $\mu$ based on the observed data $x_1, ..., x_n$
- Roughly speaking, we do this by:
1. Computing the value of the estimate, $\bar{x}$
2. Computing the value of the estimated standard error $\frac{s}{\sqrt n}$
3. See if the discrepancy $\bar{x} - \mu_0$ is "large" compared to the standard error
- We will look at various procedures using the:
	- T-test
	- Confidence Intervals
	- Rejection Regions
- Question, what is $\mu_0$? and how do we come out up with it?
- Is $\mu$ and $\bar{X}$ the same? and how to we get each one and what are they?

#### What kind of discrepancies are of interest?
When calculating $\bar{x} - \mu_0$, we are interested about the discrepancies in the positive, negative or both directions. Another way yo think about it is that given a fixed $\mu_0$, of interested and an observed sample mean $\bar{x}$, are we interested in:
1. Is $\bar{x}$ significantly *more* than $\mu_0$? (one-sided)
2. Is $\bar{x}$ significantly less than $\mu_0$? (one-sided)
3. Is $\bar{x}$ significantly different to $\mu_0$ (two-sided)

### Beer Contents
A content of a six bottle pack of beer consist of:
```
x = c(374.8, 375.0, 375.3, 374.8, 374.4, 374.9)
mean(x) # 374.8667
sd(x) # 0.294
```
In the example above, consumers will only be interested if $\bar{x}$ is significantly *less* than 375
- In this case, consumers are concerned if the company is "ripping consumers off"

However for the *producers* are concerned with both positive and negative discrepancies because:
- Consumers will be unhappy if they are underfilling
- The company will be "wasting" if they are overfilling

## Two-sided discrepancies of interest
When we are interested of the two-sided discrepancies, we are interested if whether the $|\bar{x} - \mu_0|$ is large compared to the standard error $\frac{s}{\sqrt n}$

There are two approaches to this question and we can either do the t-test approach or the confidence interval approach

In the t-test approach, we will declare a hypothesized mean value $\mu_0$ not plausible if
$$
|\bar x-\mu_0|> c \frac{s}{\sqrt{n}},
$$
for some suitable chosen $c$
- What this is basically saying is that given a suitable constant $c$, if the difference between the observed mean and the given $\mu_0$, is so large such that given a constant $c$ is multiplied to the standard error is still larger, the hypothesized mean value $\mu_0$ is not plausible

Set of plausible values for the unknown $\mu$ is:
$$
\bar x \pm c \frac{s}{\sqrt{n}}\,,
$$
For some suitable chosen $c$
- What this is saying is that the formula above sets an interval from a lower bound to an upper bound given how tight or loose given a constant $c$ which is appropriate for the situation and this interval is the ranges of possible values for the unknown $\mu$

The constant $c$ can be chosen in a sensible way in each context, the t-test or the confidence interval method
- For the t-test method, we need to control the **false alarm rate**
- For the confidence interval method, we need to control the **coverage probability**
	- **Coverage probability** is commonly also called the *confidence level* and is expressed as a *percentage*

### False Alarm Rate
A "false alarm" is when we reject incorrectly or in other words, it is when we "reject a given value $\mu_0$" when we shouldn't
- The false alarm rate is also called the significance level
- Or in other others, when we declare $\mu_0$ "not plausible" when it is in fact the true value
To control the false alarm rate, we pick a small $\alpha$ which is between (0,1) for the desired "false alarm rate"
- This $\alpha$ is usually 0.05 or 0.01

What we want is that we select a specific $c$ such that it is possible (if possible) that:
$$
P\left(|\bar X-\mu_0|>c \frac{S}{\sqrt{n}}\right)=\alpha\,.
$$
If it isn't possible that we can select a specific $c$ to fulfills this condition then select a $c$ such that the probability does not exceed $\alpha$

### Normal population: use the $t$-distribution
Under the special statistical model where the data are modelled as iid **normal random variables**, we know that **if the true population mean is indeed $\mu_0$** then the ratio:
$$
\frac{\bar X-\mu_{0}}{S/\sqrt{n}}\sim t_{n-1}\,,
$$
and by modifying the probability equation from the topic above, we can transform it to:
$$
\begin{equation}
P\left(|\bar X-\mu_0|>c \frac{S}{\sqrt{n}}\right)=
P\left(\frac{|\bar X-\mu_0|}{S/\sqrt{n}}>c\right)=P(|t_{n-1}|>c)=\alpha\,.
\end{equation}
$$

### Finding quantiles in R
In R, we can get the quantiles from distribution with the general syntax `qDISTRIBUTION()`, so for the t-distribution we use `qt`, for the normal distribution `qnorm`, etc.

### Using `qt()`
Note that if $P(|t_{n-1}|>c)=\alpha$ then:
$$
P(|t_{n-1}|\leq c)=P(-c\leq t_{n-1}\leq c)=1-\alpha
$$
- The probability of getting an outcome from a t distribution less than or equal to $c$ is $1 - \alpha$
- 
and 
$$
P(t_{n-1}<-c)+P(t_{n-1}>c)=2P(t_{n-1}>c)=\alpha
$$
So, $P(t_{n-1}>c)=\frac{\alpha}{2}$ or equivalently, $P(t_{n-1}\leq c)=1-\frac{\alpha}{2}$
- For example, if we have:
	- $\alpha = 0.05$, we need $c$ such that $P(t_{n-1}\leq c)=1-0.025=0.975$, hence `c = qt(0.975, df = n-1)`
	- $\alpha = 0.01$, we need $c$ such that $P(t_{n-1}\leq c)=1-0.005=0.995$, hence `c = qt(0.975, df = n-1)`
What this section is basically saying that to find c, we figure out what $\alpha$ we want and to calculate $c$ we find at what quantile is $\alpha /2$ or $1 - \alpha / 2$ depending if we want the upper or lower tail

### Beer Example
Recall that in our observations for the beer example is that:
- `x = c(374.8, 375.0, 375.3, 374.8, 374.4, 374.9)`
Here we have a sample size of $n = 6$ and so:
- $\alpha = 0.05$, we need $c$ such that $P(t_5 \geq c) = 0.975$
	- `qt(0.975, df = 5) # 2.57`
- $\alpha = 0.01$, we need $c$ such that $P(t_5 \geq c) = 0.995$
	- `qt(0.995, df = 5) # 4.03`
We can find the sample mean, standard error, and the discrepancy from the "given value", 375, is:
```
xbar = mean(x) # 374.8667
se = sd(x)/sqrt(6) # 0.12
discrep=abs(xbar-375) # 0.133
```
As we can see, our observed discrepancy is only just above 1 (estimated) standard error and we can see that the discrepancy is below the 2.57 standard error that we calculated when $\alpha = 0.05$ hence we retain $H_0$ so 375 is a plausible value

### Coverage Probability
For a **confidence interval**, the **coverage probability** is simply the probability that the "true" value of the unknown parameter lies within the computed confidence interval
- When you hear about a "95% confidence interval", the coverage probability is 0.95 or 95% which means that if you were to repeatedly compute confidence intervals from many samples, about 95% of those intervals would contain the true parameter

### Equivalent to False Alarm Rate Condition for t-test

The coverage probability is equivalent to the statement to the **false alarm rate** condition for the t-test (for the same $\alpha$)

Which means that if we want to have a coverage probability of 0.95 ($\alpha = 0.05$) then we need a $c$ such that:
$$
P(t_{n-1}\leq c)=1-0.025=0.975\,,
$$
```
c_95 = qt(0.975, df = 5)
c_95 # 2.57
xbar + c(-1,1) * c_95 * se
#374.56, 375.18
``` 
Or if we want a 0.99 coverage probability ($\alpha = 0.01$) then we need a $c$ such that:
$$
P(t_{n-1}\leq c)=1-0.005=0.995\,,
$$
```
c_95 = qt(0.975, df = 5)
c_95 # 2.57
xbar + c(-1,1) * c_99 * se
# 374.38, 375.35
```
Note that the confidence intervals above includes 375 which thus it is consistent with out 0.05 false-alarm rate or we can say that we should retain the null hypothesis

### Using t.test()
All of the manual computations above can be simply using with the r function `t.test` which works like the following
`t.test(x, mu=375)` or if we want to set a confidence level `t.test(x, mu=375, conf.level=0.99)`

## One-sided discrepancies of interest
The "two-sided" approach that we just outlined would be of interest to beer producers but not necessarily to beer consumers
- Consumers just want to know if they are getting ripped off or not
To consider the POV of the consumers, for our t-test, lets declare that for some suitable value of $c$:
- $\mu_0$ is not plausible if $\bar x - \mu_0 < -c \frac{s}{\sqrt{n}} \Leftrightarrow \bar x < \mu_0-c \frac{s}{\sqrt{n}}$
In the confidence interval approach, the set of plausible values for unknown $\mu$ are those "not too much bigger than $\bar{x}$" or in mathematical terms:
$$
\bigg(-\infty, \bar x + c\frac{s}{\sqrt{n}} \bigg]
$$
for a "suitably chosen" constant $c$
- The upper endpoint is sometimes called the "upper confidence limit"
- It can be interpreted as the "largest value consistent with the data"

### Controlling the (one-sided false alarm rate)
When controlling the one-sided false alarm rate, we will do something similar to the two-sided case but there is a CRUCIAL DIFFFERENCE!

Under the iid normal mode, $T = \dfrac{\bar X-\mu}{S/\sqrt{n}}\sim t_{n-1}$
And we choose $c$ so that if $\mu_0$ is the true value:
$$
P \left( \bar X < \mu_0 - c \frac{S}{\sqrt{n}}\right) = P \left( \frac{\bar X-\mu_0}{S/\sqrt{n}}< -c\right)=P(t_{n-1} < -c)=\alpha\,.
$$
By symmetry we also have $P(t_{n-1} > c) = a$ or $P(t_{n-1} \leq c) = 1 - a$

For the false alarm rate,
- for when $\alpha = 0.05$ we need a $c$  such that $P(t_{n-1}\leq c)=1-0.05=0.95$
- For when $\alpha = 0.01$ we need a $c$ such that: $P(t_{n-1}\leq c)=1-0.01=0.90$

Notice here that the difference between the one-sided and the two sided is that we don't have to divide our $\alpha$ by two because we are only looking at one tail of the t-distribution
- Also hence why in one-sided t-distribution our confidence intervals are from $-\infty,\bar{x}+c\frac{s}{\sqrt{n}}$ or $\bar{x}-c\frac{s}{\sqrt{n}},\infty$ and in two-sided there is no infinity

### Beer example
For the $\alpha = 0.05 \ or  \ 0.01$ false alarm rate, since $n = 6$,
```
c_05 = qt(0.95, df = 5) # 2.01 - When alpha = 0.05
c_01 = qt(0.99, df = 5) #3.36 - When alpha = 0.01
```
Note that the values of the one-sided versions are much smaller than the two-sided version which makes the one-sided test "more sensitive" than two-sided versions

For the confidence intervals for our one-sided t-test, we would calculate normally as we would in the two-sided but with our new $\alpha$
```
c_05 = qt(.95, df = 5)
c(-Inf, xbar + c_05 * se) # -inf, 375.11

c_01 = qt(.99, df = 5)
c(-Inf, xbar + c_01 * se) #375.27
```
- These two intervals both contain 375 which makes 375 a plausible value thus retaining the null hypothesis that the population mean for many milliliters of beer is in a bottle is indeed 375ml

### Using t.test()
This can all be done again using the R function `t.test()` by:
```
t.test(x,mu=375, alternative = "less")
t.test(x,mu=375, alternative = "less", conf.level = 0.99)
```

### Observed Significance Level: The P-value
The observed significance level or p-value is the value of $\alpha$ for which the observed data is "right on the edge"

More formally it is the smallest false alarm rate for which we would "reject" a given $\mu_0$ or when the non-coverage probability (1-confidence level) for which $\mu_0$ is on the boundary of the confidence interval

## Rejection Regions
When we are testing a hypothesis, it is important to keep a **decision rule** in mind so that we can reject our null hypothesis, $H_0$ and our decision rule was then when the p-value is less than a certain fixed preassigned level, p-value $\leq \alpha$ where $\alpha = 0.05, 0.10,$ etc.
- We reject or do not reject $H_0$ according to whether the p-value is less than $\alpha$ or greater than $\alpha$
	- We reject $H_0$ when the p-value is less than $\alpha$
	- We retain $H_0$ when we p-value is greater than $\alpha$
- $\alpha$ is called the significance level of the test, which is the boundary between rejecting and not rejecting $H_0$

