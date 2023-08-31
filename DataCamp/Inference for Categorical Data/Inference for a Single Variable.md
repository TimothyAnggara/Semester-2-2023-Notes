## **General Social Survey**
- Rich resource of categorical data
- It is a survey done in America where they ask people several questions with categorical responses
- Let's do an example using one of the categorical responses, Happy.

First, let's try to summarize the happy variable to a single proportion, p_hat

```
p_hat <- gss2016 %>% summarize(prop_happy = mean(happy == "HAPPY")) $>$ pull()
```

The code gives us 0.77 which tells us that around 77% of all Americans are happy from the people that replied the GSS survey

Because the statistic that we received is from a sample from the entire population, we can't be too sure about our proportion. We can create a confidence interval by adding and subtracting two standard errors from p_hat

$$
(\hat{p} - 2 \times SE, \hat{p} + 2 \times SE )
$$
We can find the standard error using bootstrap and finding the standard deviation of the bootstrap distribution

## **How to do the bootstrap**
1. Specify the variable that are interested in using **specify()**
2. Draw samples from that variable with replacement with the same size as our original dataset using **generate()**
3. For each replicate, we calculate the statistic we are interested in using **calculate()**
4. We can then look at the distribution using **ggplot()**
5. The standard deviation of the distribution is a good estimate for the standard error

```{r}
library(infer)
boot <- gss2016 %>% 
	specify(response = happy, success = "HAPPY") %>%
	generate(reps = 500, type = "bootstrap") %>%
	calculate(stat = "prop")

ggplot(boot, aes(x = stat)) + geom_density()
SE <- sd(stat)
CI <- c(p_hat - 2 * SE, p_hat + 2 * SE)
```

## Interpreting a Confidence Interval

From the code above, we can conclude that the true proportion of Americans that are happy are between 0.705 and 0.841. But what do we mean by confident?

Confidence means how likely we are that the interval contains the true value of an unknown population parameter, in our example happiness

Width of the interval is affected by:
- n
	- Less n --> wider interval
- confidence level
	- Smaller confidence level --> wider interval
- p
	- 

## Approximation shortcut

- Standard errors increase when
	- n is small
	- p is close to 0.5

When we are calculating the standard error, we have been using bootstrapping but there is another way using the normal distribution. This other method is just an approximation

We can only use this method if observations are independent and n is sufficiently large. When using this method, we can use this formula to find the standard deviation:

$$
\sqrt{\frac{\hat{p}\times(1-\hat{p})}{n}}
$$

To know if our data is sufficiently large, we can check by:

$$
n\times \hat{p} \geq 10 
$$
$$
n \times (1-\hat{p}) \geq 10
$$


Lets try using this approximation to find the standard error

```{r}
p_hat <- gss2016 %>% 
	summarize(mean(happy == "HAPPY") %>% pull())
n <- nrow(gss2016)

SE_approx <- sqrt(p_hat * (1 - p_hat) / n)
```

We'll find that if the given assumptions are satisfied, the normal approximation and the computational value of the standard errors are remarkably similar.