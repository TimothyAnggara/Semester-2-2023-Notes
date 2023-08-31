
### Learning Objectives:
- Paired $t$-tests
- Checking for normality
- Sign test
- Additional practice questions

## Paired $t$-tests (Revision)
### Rats
- Does biochemical substance have an inhibitive effect on muscular growth
For each of 10 rats:
- One hind leg muscle was regularly injected with the biochemical substance
- The other leg was regularly injected with a harmless placebo
- At the end of 6 months the weights of the muscles were measured and recorded:

| Rat ID              | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  |
| ------------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Biochemical Leg (g) |   1.7  |  2.0   |  1.7   |  1.5   | 1.6    |  2.4   |  2.3   | 2.4    |  2.4   |  2.6   |
| Placebo leg (g)                    |   2.1  |    1.8 |  2.2   |  2.2   | 1.5    |  2.9   |   2.9  |   2.4  |  2.6   | 2.5    |
```
rat = data.frame(
  bio = c(1.7, 2.0, 1.7, 1.5, 1.6, 
          2.4, 2.3, 2.4, 2.4, 2.6),
  pla = c(2.1, 1.8, 2.2, 2.2, 1.5, 
          2.9, 2.9, 2.4, 2.6, 2.5)
) %>% mutate(d = pla - bio)
rat %>% summarise(mean = mean(d), sd = sd(d))
# mean = 0.25 sd = 0.33

t.test(rat$d, alternative = "greater")
```

## Checking for Normality
In many test, we consider the assumption that our data are sampled from a normal distribution.
- If you have a large enough sample, then the normality assumption is not as important as you can rely on the central limit theorem
- In small samples, it is difficult to tell if our sample comes from a normal distribution
We can best test normality using QQ plots but Boxplots also works but not the best

![[Pasted image 20230828223441.png]]
![[Pasted image 20230828223529.png]]
To check for normality in a boxplot, we are mostly looking for **symmetry** but this only works if we have large samples. When we have smaller samples it becomes more difficult to check if our samples actually come from a normal distribution

With QQ plots, it is more clear whether our sample comes from a normal distribution because we are comparing our data points to a QQ line.
![[Pasted image 20230828224227.png]]
![[Pasted image 20230828224236.png]]
So, with QQ plots, if we want to test whether our samples is normal or not, we can just create some QQ plots that comes from a normal distribution with the same sample size as our sample, and compare those QQ plots with a QQ plot we generated with our own sample data.
## Sign test
In a sign test, we want to test whether our $H_0 : \mu = \mu_0$
- We want to test if our sample mean is greater, less than, or not equal to the hypothesized mean
Suppose a sample $X_1, ..., X_n$ are independently samples from a continuous distribution with mean $\mu$

If the distribution is **symmetric** about $\mu_0$ under $H_0$, then $D_i = X_i = \mu_0$ should scatter around 0
	- In other words $D_i$ is equally likely to be positive or negative
	- If $H_0$ is true, the probability of getting a positive $D_i$ is $0.5$

The **sign test** reduces to a binomial test of proportions and is **nonparametric** test as no assumption on the data distribution is made except symmetry and independence
![[Pasted image 20230828224816.png]]

### Sign Test for Paired Data
In the case of paired data, we will first need to find the differences between the before/after trial, or left/right leg, etc. and once we have done that we can conduct a **sign test** if we do not feel comfortable with making the normality assumption, else, use a t-test
- Note that if we have any differences that are equal to 0, we will discard them
```
rat %>% mutate(
  pos_d = d > 0
) %>% arrange(d)
sum(rat$d > 0) # 6
sum(rat$d != 0) # 9
binom.test(c(6, 3), p = 0.5, alternative = "greater")
```
![[Pasted image 20230828225035.png]]

### Benefits and drawbacks of the Sign Test
The sign test **ignores a lot of the information** in the sample but it can be applied in **quite general** situations
- The sign test does not depend on the distribution of the data
	- Test that does not depend on the distribution are called **nonparametric**
If the normality assumption is satisfied, the $t$-test is more powerful
- The $t$-test is more likely to reject the null hypothesis given that it is false
The sign test can be used to test if a single sample is taken from a **continuous distribution** that is **symmetric about** its population mean $\mu$
Sign test is more robust than a $t$-test
## Additional Practice 