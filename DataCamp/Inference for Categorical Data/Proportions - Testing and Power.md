## Hypothesis Test for a Proportion

A confidence interval is formed using the standard error, which is the standard deviation of the sampling distribution
- The sampling distribution is the core of the confidence interval

The confidence intervals tells us how much uncertainty you have in your estimate

A hypothesis test on a proportion asks the question: **What sort of p-hats would you observe if the true parameter p held a particular value**

We can assert this hypothesis, this model of the state of the world, using hypothesize

So now, our steps to test this hypothesis looks like this:
1. Specify()
2. hypothesize() 
3. generate() the samples
4. calculate() the mean
5. ggplot() the distribution
6. summarize() the SD

```{r}
null <- gss2016 %>% 
	specify(
		responose = cappun,
		success = "FAVOR"
	) %>% 
	hypothesize(
		null = "point"
		p = 0.5
	) %>% generate(
		reps = 500,
		type = "simulate"
	) %>% 
	calculate(stat = "prop") 
```

## Intervals for Differences

Lets say we wanted to ask the question: **Do men and women believe at different rates whether they believed there is life after death?**

We can have our null hypothesis  and alternative hypothesis to be:
$$
H_0 : p_{female} - p_{male} = 0
$$
$$
H_1 : p_{female} - p_{male} \neq 0
$$

We can take a look at the proportions in the dataset using ggplot() or calculating the differences

```{r}
ggplot(gss2016, aes(x = sex,, fill = postlife)) + geom_bar()

p_hats <- gss2016 %>% 
	group_by(sex) %>%
	summarize(mean(postlife == "YES", na.rm = TRUE)) %>%
	pull()

d_hat <- diff(p_hats)
```

### Generating data from H0

Since our H$_0$ is saying that there is no difference between the proportion of female and male, we can say that:

- There is no association between belief in the afterlife and the sex of a subject
- The variable postlife is independent of sex

Because we can say this, we can generate data by permutation

```{r}
gss2016 %>% 
	specify(
		response = postlife,
		explanatory = sex
		success = "YES"
	) %>% 
	hypothesize(null = "independence") %>%
	generate(reps = 1, type = "permute") 
```

The code above creates a world where the null hypothesis is true (the variable postlife is independent of sex) but the postlife column stays the same

Lets now try to build a null distribution based on the method above

```{r}
gss2016 %>% 
	specify(
		postlife ~ sex #This line basically does what response and explanatory does, "I'd like to express postlife as a function of sex"
		success = "YES"
	) %>% 
	hypothesize(null = "independence") %>%
	generate(reps = 1, type = "permute") %>% 
	calculate(stat = "diff in props", order = c("FEMALE", "MALE"))
```


## Statistical Errors

Statistical errors - Errors that are made in the procedure of a hypothesis test

|           | H0 True      | H0 False |
| --------- | ------------ | -------- |
| Retain H0 | Ok           |Type II Error|
| Reject H0 | Type 1 Error |Ok|
 
### What is the probability of rejecting a true null hypothesis?

Consider the situation you are testing a null hypothesis that the difference between the two proportions is zero

We can use either normal approximation or permutation to find the null distribution

The distribution of the differences in p-hats you might observe in a given sample if the null hypothesis is true

