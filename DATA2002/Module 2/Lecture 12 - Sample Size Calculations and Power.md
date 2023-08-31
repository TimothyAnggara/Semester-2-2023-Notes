## Power and Sample Size
### Errors in Hypothesis Testing
|                    | $H_0$ True (Innocent)   | $H_0$ False (Guilty)         |
| ------------------ | ----------------------- | ---------------------------- |
| Don't reject $H_0$ | Correct decision        | Type II Error ($\beta$)      |
| Reject $H_0$       | Type I error ($\alpha$) | Correct Decision ($1-\beta$) |
|                    |                         |                              |
- Type I Errors: level of significance, $\alpha = P(reject \ H_0 | H_0 \ true)$
- Type II Errors: Call it $\beta$
- Power: $1 - \beta = P(reject \ H_0 |  H_1 \ true)$
	- $P(reject \ H_0 |  H_0 \ false)$
	- Power means the ability to correctly reject the null hypothsis
	- When we say 80% power, we mean that we are able to correctly reject the null hypothesis 80% of the time in our experiment
### General Testing Setup
- Interested in inference concerning an unknown population mean $\mu$
In this setup, lets say we are performing a two-sided t-test and we are considering a fixed value $\mu_0$ ("Hypothesized Value") and we will be finding the absolute difference of our hypothesized value to our observed data ($x_1, x_2, ..., x_n$) sample mean ($\bar{x}$) 

From the sample standard deviation $s$ we can calculate the estimated standard error $s/\sqrt n$ and we will use this to check if the observed discrepancy $|\bar{x} - \mu_0|$ is large compared to the standard error

We will "reject" the value $\mu_0$ as "implausible":
$$
\text{Reject if } |\bar x-{\mu_0}|>c \frac{s}{\sqrt{n}}\,,
$$
where c is chosen to be the false alarm rate from some fixed small value of $\alpha$

### Model Assumptions
The false alarm rate determination can only be made if a suitable statistical model is assumed for the data

### Beer Example
- Suppose we have $n = 6$ and choose a **false alarm rate** of $\alpha = 0.05$ Then the constant $c$ needs to satisfy:
$$
2P(t_5>c)=\alpha
$$
$$
P(t_5>c)=\alpha/2=0.025
$$
$$
P(t_5\leq c)=0.975\,.
$$
```
c_05 = qt(0.975, df = 5)
c_05 = 2.57
```
![[Pasted image 20230827205429.png]]

### Why allow false alarms at all?
- Why should we always set our **false alarm rate** to be 5%?
	- Why don't we make it really small? like $10^{-6}$
- Answer: Because then you would never reject anything, even if you should?
	- The technical reason is because the test would have **no power**

The **power** of a test is the probability that the test rejects the null hypothesis , $H_0$ when a **specific** alternative hypothesis $H_1$ is true
$$
Power \ = \ P(reject \ H_0 | H_1 \ is \ true)
$$
- There is an inherent trade-off between Type I and Type II errors
	- Decrease $\alpha$ naturally leads higher $\beta$ which leads to lower powe
	 - So a really small $\alpha$ will leads to a really high $\beta$ thus lower power
	 - In other words, a higher $\alpha$ means our ability to correctly reject the null hypothesis decreases
### Statistical Power in One Sample $t$-test
- The probability of "rejecting" as a function of the true population mean $\mu$ is the **statistical power function** of the test
$$
\begin{align*}
P_{\color{red}{\mu}} \left(\text{reject }H_0\right)=
P_{\color{red}{\mu}} \left( \left| \bar X-\color{blue}{\color{blue}{\color{blue}{\mu_0}}} \right| >c\frac{S}{\sqrt{n}} \right)=
P_{\color{red}{\mu}} \left( \frac{\left| \bar X-\color{blue}{\color{blue}{\color{blue}{\mu_0}}} \right|}{S/\sqrt{n}}>c \right)\,.
\end{align*}
$$
To determine this, we need to know the distribution of the $t$-statistic for testing $\mu_0$:
$$
\frac{ \bar X-\color{blue}{\color{blue}{\mu_0}} }{S/\sqrt{n}}
$$
When the true populaiton mean $\mu$ is not necessarily equal to $\mu_0$ 


### Beer Example: Power calculation
- We want to plot the power function of the test as a function of $\mu$
- First, Assume the "true" $\sigma$ is equal to the sample value and suppose that the sample sd is indicative of the "true" population standard deviation
```
x = c(374.8, 375.0, 375.3, 374.8, 374.4, 374.9)
sig = sd(x)  
```

### Power as a function of true mean $\mu$
Given that we pick $H_0 : \mu = 375$
![[Pasted image 20230827211749.png]]
The further the true mean $\mu$ is from the value $\mu_0, 375,$ picked for the null hypothesis the more likely we are to reject

### Discrepancy needed to Achieve a Certain Power
Assume:
- Population SD $\$ = 0.294
- Sample size of $n$ = 6
- $H_0 : \mu = 375$ vs $H_0: \mu \neq 375$

The question we are asking is how much lower than 375 does $\mu$ need to be for us to be 80% sure of "detecting" that $\mu \neq 375$ with a two-sided test with a false alarm rate of 0.05
- $\mu$ value with power 80% is around 374.6

For a two sided test with n = 6 and a false alarm rate of $\alpha = 10^{-6}$, we need a critical value of 28.476
```
crit_val = qt(1 - (1e-6) / 2, df = 5)
crit_val #28.478
```
We would need a discrepancy equal or more than 28 standard errors before we would reject 375

### Power as a function of $\alpha$
To achieve a power of about 80% with a small false alarm rate, we would require a true $\mu$ lower than 371 or more than 379
![[Pasted image 20230828002746.png]]

### Power as a function of $n$


## How to do it in R
### There is a package for that
- `library(pwr)`
	- pwr.t.test()
	- `pwr.t.test(n = NULL, d = NULL, sig.level = 0.05, power = NULL, type = c("two.sample", "one.sample", "paired"), alternative = c("two.sided", "less", "greater"))`
	- One-sample, two-sample, and paired t-test with equal numbers of samples
	- pwr.t2n.test()
	- `pwr.t2n.test(n1 = NULL, n2 = NULL, d = NULL, sig.level = 0.05, power = NULL, alternative = c("two.sided", "less","greater"))`
	- Two sample t-test with unequal number of samples

### Cohen's d
- The pwr function take an input "Cohen's d" which is a number that tells how the size of the difference between two groups
$$
d = \frac{|\mu_1 - \mu_2|}{\sigma}
$$
### Beer Examples
Suppose the population has a standard deviation 0.294, with a sample size of $n$ = 6. How much lower than 375 does $\mu$ need to be for us to be 80% sure of "detecting" that $\mu \neq 375$ with a two-sided test which has a false alarm rate of 0.05?
```
res = pwr.t.test(n = 6, d = NULL, sig.level = 0.05, power = 0.8, type = "one.sample", alternative = "two.sided")
res

res$d * 0.294 # d * sigma gives the difference between means
```

Suppose that $\mu = 374.87$ and standard deviation = 0.294, what sample size $n$ would be needed to be at least 80% sure of detecting that $\mu \neq 375$ with a two-sided test which has false alarm rate 0.05?
```
res = pwr.t.test(n = NULL,
                 d = (374.87-375)/0.294, 
                 sig.level = 0.05,
                 power = 0.8, 
                 type = "one.sample", 
                 alternative = "two.sided")
res
```
We need at least 43 observations 

Independence means we are testing if the variables are independent or not

Goodness of fit - 