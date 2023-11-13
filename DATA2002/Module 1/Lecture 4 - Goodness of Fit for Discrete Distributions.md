## Radiation Exposure
In biological dosimetry, the goal is to estimate the dose of **ionizing radiation** absorbed by an exposed individual by using chromosome damage in peripheral lymphocytes

Damage in DNA from radiation is randomly distributed between cells producing chromosome aberrations. The outcome of interest is the number of aberrations observed

The aberrations typically follows a Poisson distribution where the rate is dependent on the dose

The table below shows the number of chromosome aberrations from a nuclear accident

| Number of Aberrations | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
| --------------------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Frequency             | 117 | 94  | 51  | 15  | 0   | 0   | 0   |  1   |

## Poisson Distribution

Poisson random variable represents the probability of a given number of events occurring in a fixed period if these events occur independently with an average rate $\lambda$

If $X$ is a Poisson random variable with a rate parameter $\lambda$, the probability mass function is:

$$
P(X = k) = e^{-\lambda}\frac{\lambda^k}{k!}, k = 0,1,2,...
$$

## Chi-Squared test for Discrete Distribution

Suppose we have a sample of observations and we want to test whether the sample is taken from a population with a given distribution function $F_0$ 

To do this, we can summarize the observed data by tabulating the observed frequencies $y_i$, and for each possible outcome, compare with the expected frequencies, $e_i$ calculated using the expected probabilities $p_i$ from the hypothesized distribution function $F_0$

The general chi-squared goodness-of-fit test with test statistic,

$$
T = \sum^k_{i=1}{\frac{(Y_i-e_i)^2}{e_i}} = \sum^k_{i=1}{\frac{(Y_i-np_i)^2}{np_i}}
$$

Before we get into finding the test statistic, it's important to outline what our hypothesis are:
- **Null Hypothesis:** Our data follows the Poisson distribution
- **Alternate Hypothesis:** Our data does not follow the Poisson Distribution

Since we don't know the expected probabilities $p_i$ are replaced be their estimates $\hat{p}_i$ which is found using:
$$
P(X = k) = e^{-\lambda}\frac{\lambda^k}{k!}, k = 0,1,2,...
$$

We can estimate $\lambda$ by the sample mean: 
```{r}
number_of_abberations <- c(0,1,2,3,4,5,6,7)
frequency <- c(117, 94, 51, 15, 0, 0, 0, 1)
lambda <- (number_of_abberations * frequency) / sum(frequency) # 0.89
```
![[Pasted image 20230807203552.png]]

Before we continue and calculate the p-value, we can see a very big error, starting from 4-7, we can see that the expected count is less than 5 which is a problem.

We can combine 4-7 such that the expected count of the groups is $\geq 5$ 

![[Pasted image 20230807203746.png]]
Here, the final observed test statistic is, $t_0 = 0.08 + 0.57 + 0.71 + 0.07 = 1.43$
From the table above, since we have 4 categories, 0,1,2,3, $\geq 4$ we have 4 groups and we have one estimated parameter, $\hat{p}$. So our degrees of freedom is $4 - 1 - 1 = 2$ 

```{r}
pchisq(1.43, df = 2, tail.lower = FALSE) # 0.49
```

Because our p-value is greater than 0.05, we do not reject the null hypothesis and say that our data is consistent with the Poisson distribution

```{r}
y = c(117, 94, 51, 15, 0, 0, 0, 1) # input the observed counts
x = 0:7 # define the corresponding groups
n = sum(y) # total number of samples (sample size)
k = length(y) # number of groups
(lam = sum(y * x)/n) # estimate the lambda parameter
# 0.89

p = dpois(x, lambda = lam) # obtain the p_i from the Poisson pmf
p 
# 4.097999e-01 3.655769e-01 1.630631e-01 4.848878e-02
# 1.081404e-02 1.929412e-03 2.868670e-04 3.655859e-05

p[8] = 1 - sum(p[1:7]) # redefine the 8th element P(>=7) NOT P(7)
round(p, 5)# Round p to 5 decimal places

ey = n*p
# 113.92436722 101.63037076 45.33153228 13.47988010 
# 3.00630420  0.53637658 0.07974904 0.01141984

ey >= 5
# TRUE TRUE TRUE TRUE FALSE FALSE FALSE FALSE
```

Because we have some expected frequency not satisfying the assumptions we need to couple some of the categories

```{r}
yr = c(y[1:3], sum(y[4:8])) #Reduced category counts
# 117 94 51 16
eyr = c(ey[1:3], sum(ey[4:8])) #Reduce category expected cell count
# 113.92437 101.63037 45.33153 17.11373
all(eyr >= 5) # True
pr = c(p[1:3], sump[4:8]) #Reduced category hypotized probabilities
# 0.40979988 0.36557687 0.16306307 0.06156018
kr = length(yr)
t0 = sum((yr - eyr)^2 / eyr) # 1.43
pchisq(t0, df = kr - 1 - 1) #0.487
```



1. Is this a random sample of DATA2X02 students?
    
2. What are the potential biases? Which variables are most likely to be subjected to this bias?
    
3. Which questions needed improvement to generate useful data