## Genetic Linkage

In a backcross experiment to investigate the genetic linkage between two genes A and B in a species of flower with 400 offspring.

| Phenotype | AB  | Ab  | aB  | ab  |
| --------- | --- | --- | --- | --- |
| Count     | 128 | 86  | 74  | 112 |

- A - Pink flowers
- a - Yellow flowers
- B - Smooth leaves
- b  - wrinkled leaves

Under the no linkage model, the four phenotypes are equally likely so we would see equal proportions for each Phenotype

| Phenotype | Expected Proportion |
| --------- | ------------------- |
| AB        | 1/4                 |
| Ab        | 1/4                 |
| aB        | 1/4                 |
| ab        | 1/4                 |

If linkage is in the coupling phase, the probabilities of the four phenotypes are given by:

| Phenotype | Expected Proportion |
| --------- | ------------------- |
| AB        | $\frac{1}{4}(1-p)$  |
| Ab        | $\frac{1}{2}p$      |
| aB        | $\frac{1}{2}p$      |
| ab        | $\frac{1}{4}(1-p)$  |

where $p$ is the **recombination fraction** and is something we will need to estimate $p$ as the overall proportion of observed Ab and aB

## Hypothesis Testing (Recap)

### Hypothesis
- Statement against which we search for evidence is called the null hypothesis
	- Denoted as $H_0$ 
	- The "no difference" statement

### Assumptions
- Observations are assumed to be independent of each other and chosen at random
- Each test we consider will have its own set of assumptions

### Test Statistic
- Since observations vary from sample to sample, we can never be sure whether $H_0$ is true or not
	- We may state that our $H_0$ : there is no difference between the proportions of different phenotypes and lets say that after a hypothesis test we get a p-value of 0.7.
	- We must say that we **retain** the null hypothesis and not accept it as the proportion of phenotypes may be a mix of independent distribution + something else
- A test statistic is a function of the observations, $T = f(X_1, ..., X_n)$  such that the distribution of $T$ is known assuming $H_0$ is true
	- The test statistic can be used to test if the data are consistent with $H_0$
- The **observed test statistic**, $t_0$, is where we plug our observed data into the formula for the test statistic
- A large $t_0$ value is taken as evidence of poor agreement with $H_0$

### Significance
- The p-value is defined as the probability of getting a test statistic $T$, as or more extreme than the value we observed, $t_0$, assuming that $H_0$ is true

### Decision
An observed large positive of negative test statistic and hence small p-value is taken as evidence of poor agreement with $H_0$

- If the p-value is small, then either $H_0$ is false or $H_0$ is true and the poor agreement is due to an unlikely event
- A large p-value does not mean there is evidence that the null hypothesis is true


## No Linkage Model

**Null Hypothesis:** Each of the phenotypes are equally likely
**Alternative Hypothesis:** The phenotypes are not equally likely

Under the null hypothesis, the counts are uniformly distributed across the 4 categories,
$$
p_i = 0.25 \space for \space all \space i
$$
```{r}
library(tidyverse)
df = tibble(
	phenotype = c("AB", "Ab", "aB", "ab"),
	y = c(128, 86, 74,112),
	p = c(1/4, 1/4, 1/4, 1/4),
	e = sum(y) * p
)

# A tibble: 4 × 4 
phenotype  y  p  e 
<chr> <dbl> <dbl> <dbl> 
1 AB 128 0.25 100 
2 Ab 86 0.25 100 
3 aB 74 0.25 100 
4 ab 112 0.25 100
```

### Test statistic
We need to ask the question: "How big a difference is the distances between the expected and the original data and is it significant?"

To answer this question, we can use the chi-squared test statistic which is:

$$
t_0 = \sum^k_{i=0}{\frac{(y_i - e_i)^2}{e_i}}
$$
where k is the number of categories (groups)

```{r}
df %>% 
	mutate(squared_discrepency = (y-e)^2, 
	contribution = (y-e^2)/e
	)

t0 = sum(df$contribution)
#18
```

Now let's try to create a world where the null hypothesis is true, i.e. The difference between the proportions of phenotypes are 0

```{r}
n = 400
phenotype = c("AB", "Ab", "aB", "ab")
no_link_p = c(1, 1, 1, 1)/4
e = n * no_link_p
set.seed(1)
sim1 = sample(
  x = phenotype,
  size = n, 
  replace = TRUE, 
  prob = no_link_p

table(sim1)
# sim1 
# ab aB Ab AB
# 88 121 96 95
)
```
For this simulation, if we compute the test statistic,

```{r}
sim_y = table(sim1)
sum((sim_y - e)^2/e)
# 6.26
```
We get 6.26 which is a lot smaller than the test statistic we got from our **actual** data, 18

Perhaps this may be a lucky fluke so lets try and create 3000 simulations and create a histogram for each of the simulation's test statistic

```{r}
B = 3000
sim_t_stats = vector(mode = "numeric", length = B)
for(i in 1:B){
  sim = sample(x = phenotype, size = n, 
               replace = TRUE, prob = no_link_p)
  sim_y = table(sim)
  sim_t_stats[i] = sum((sim_y - e)^2/e)
}
hist(sim_t_stats, main = "", breaks = 20)
```
![[Pasted image 20230807193723.png]]

### Simulate
- Now we have a good idea of the shape of the **distribution** of the test statistic **when the null hypothesis is true**
- We can compare the test statistic we calculated on the original data to the **null distribution**
	- The histogram that we created
- One way to do this is to ask the question:
	- Given that the **null hypothesis is true,** how likely is it that we observe a test statistic as or more extreme than that we calculated from our original sample?

```{r}
mean(sim_t_stats >= t0) #or
sum(sum_t_stats >= t0) / B

# 0.001
```
So in, 0.1% of the 3000 samples where the **null hypothesis is true**, we got a simulated sample that was "equal to or more extreme" than our original sample 

### Using a Chi-Square Test

Instead of having to do a simulation every time to find our null distribution, we can use a chi-squared test

### $X^2$ Test Degrees of Freedom

The degree of freedom is $k-1-q$ because the -1 comes from the thing we are estimating, in this case the hypothesis. The $q$ comes from the number of parameters that need to be estimated from the sample
- In the no linkage example, $q$ = 0 because we did not estimate any parameters
	- All the hypothesized proportions are 0.25

Using the Chi-Squared Distribution will only be accurate if the **expected frequency** is sufficiently large, $e_i \geq 5$, and that each observation is independent of each other
- If the expected frequency is not $\geq 5$ then we need to pool adjacent categories so that the expected frequencies are always $\geq 5$ 

## Table for Calculating the Test Statistic
![[Pasted image 20230807194926.png]]
- $y_i$ are the observed counts
- $p_{i0}$ are the hypothesized probabilities
- $e_i$ are the expected counts assuming the **null hypothesis is true**
- $t_0$ is the observed test statistic

### No Linkage model
![[Pasted image 20230807195037.png]]

#### HATPC
- Hypothesis
	- **Null Hypothesis**: No linkage
		- $p_i$ = $\frac{1}{4}$, $for\space all \space i$
	- **Alternative Hypothesis**: Linkage exist
		- $p_i \neq \frac{1}{4}, for\space all \space i$  
```{r}
df
# A tibble: 4 × 4 
phenotype  y  p  e 
<chr> <dbl> <dbl> <dbl> 
1 AB 128 0.25 100 
2 Ab 86 0.25 100 
3 aB 74 0.25 100 
4 ab 112 0.25 100

no_link_p <- c(0.25, 0.25, 0.25, 0.25)
expected_counts <- n * no_link_p # 100,100,100,100
```
- Assumptions
	- Independent observations
	- Expected counts $\geq 5$
```{r}
expected_counts >= 5
# True True True True

all(expected_counts >= 5)
# True
```
- Test Statistic
```{r}
t0 <- sum((y-e)^2/e)
# 18
```
- P-value
$$
P(T \geq t_0) = P(X^2_3 \geq 18) = 0.0004
$$
```{r}
pchisq(t0, df = 3)
# 0.00439...
```
- Conclusion / Decision
Since P-value is much smaller than 0.05, there is strong evidence against $H_0$. thus we reject the no linkage model and $H_0$

## Linkage Model

Under the coupling phase linkage mode, the probabilities of each of the four phenotypes outcomes are given by:

| Phenotype | Expected Proportion |   Observed Count  |
| --------- | ------------------- | --- |
| AB        | $\frac{1}{4}(1-p)$  |  128   |
| Ab        | $\frac{1}{2}p$      |   86  |
| aB        | $\frac{1}{2}p$      |   74  |
| ab        | $\frac{1}{4}(1-p)$  |   112  |

To be able to test this mode, we need to first estimate the parameter p from the observed data which is $\hat{p}$, the proportion of observed offspring with phenotype Ab or aB

$$
\hat{p} = \frac{86+74}{400} = 0.4
$$
$$
p_{10} = p_{40} = \frac{1}{2}(1-0.4) = 0.3
$$
$$
p_{20} = p_{30} = \frac{1}{2}0.4 = 0.2
$$

```{r}
count = c(128, 86, 74,112)
n = sum(y)
link_p = c(0.3,0.2,0.2,0.3)
expected_count = link_p * n
t0 = sum((count - expected_count)^2/expected_count) # 1.96
```

We can simulate the null distribution and here it is important to note that re-calculate the recombination fraction every time because the recombination fraction is dependent on the sample at the time. 

```{r}
B = 3000
sim_t_stats = vector(mode = "numeric", 
                     length = B)
for(i in 1:B){
  sim = sample(x = phenotype, size = n, 
               replace = TRUE, prob = link_p)
  sim_y = table(sim)
  # estimate recombination fraction
  p_e = sum(table(sim)[2:3])/n
  # calculate expected cell counts
  e = 400 * c(1 - p_e, p_e, p_e, 1 - p_e)/2
  sim_t_stats[i] = sum((sim_y - e)^2/e)
}

mean(sim_t_stats >= t0) # 0.382
```
We can see that the p-value that we got from this null hypothesis is 0.382, which is greater than our alpha which means that we retain our null hypothesis and there is evidence that the data is consistent with the "coupling phase" linkage model

### To do this without simulation

```{r}
n = 400
y = c(128, 86, 74, 112)
n = sum(y)
link_p = c(0.3, 0.2, 0.2, 0.3)
ey = link_p * n
t0 = sum((y - ey)^2/ey) # 1.966667

ey >= 5 # TRUE TRUE TRUE TRUE

pchisq(t0, df = k-1-1, lower.tail=FALSE) # 0.3741

```