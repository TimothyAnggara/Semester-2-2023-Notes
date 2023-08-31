
## Case Study - Election Fraud - Goodness of fit

Lets introduce the goodness of fit test in the context of a case study on election fraud

Lets say we had an election and one of the officers had stuffed the ballot boxes with fake ballots

**Original ballot**

| Precinct  | Candidate 1 | Candidate 2 |
| -------  | ----------- | ----------- |
| 1        | 53          | 37          |
| 2       | 36          | 31          |
| 3       | 68          | 55          |
| 4       | 17          | 19          |
| 5       | 24          | 27          |


**Modified Ballot**

| Precinct  | Candidate 1 | Candidate 2 |
| -------  | ----------- | ----------- |
| 1        | 53          | 77          |
| 2       | 36          | 31          |
| 3       | 68          | 85          |
| 4       | 17          | 19          |
| 5       | 24          | 27          |

Lets say for our example, these numbers were altered before reporting to the authorities which prove to be difficult to detect but we can take a look to a statistical method called **Benford's Law**

### Benford's Law A.K.A "the first digit law"

This law appears when looking at broad collection of numbers and paying attention to the first digit of each number

We can take a look the first digits of the population of all the countries in the world. From the gapminder library, we can see that the distribution of the countries first digits are not actually uniformly distributed.

![[Pasted image 20230806234449.png]]

If the election was fair, then vote counts should follow Benford's Law and if not, the election may be fraudulent as they do not following Benford's law

## Goodness of Fit

If we take a look at the count of Iran's election data, we can see that there are a lot of twos and that there are more 7s than 6s

![[Pasted image 20230807133548.png]]

Now, we want to know if whether these deviations from Benford's law due to random chance or if Benford's Law is not a good description of the data

To answer this question, we need a test statistic to measure how far the vote distribution is from Benford's Law and to do this we can use the Chi-squared distance 

$$
\sum{\frac{(O_i - E_i)^2}{E_i}}
$$

![[Pasted image 20230807135008.png]]
 
### Example with GSS data 

Let try to find if the distribution of political parties in the US is uniform

First we can create a plot to see what our current distribution of parties look like and lets create a line to see where the bars should be if they were uniform

```{r}
ggplot(gss2016, aes(x = party)) + 
	geom_bar() +
	geom_hline(yintercept = 149/3, color="goldenrod", size = "2")
```

| Dem | Ind | Rep |
| -------- | -------- | -------- |
| 43   | 72  | 34   |

Once we created our plots, lets create a table of the counts in the bar chart and create a vector with our hypothesized probabilities and do a chi-squared test

```{r}
tab <- gss2016 %>%
	select(party) %>%
	table()

p_uniform <- c(Dem = 1/3, Ind = 1/3, Rep = 1/3)
chisq.test(tab, p = p_uniform)$stat
## X-Squared = 15.879
```

Once we get our Chi-Squared statistic, we then need to figure out if this is a big distance? And to answer this question we can do a hypothesis test

```{r}
sim_1 <- gss2016 %>%
	specify(response = party) %>%
	hypothesize(null = "point", p = p_uniform) %>%
	generate(reps = 1, type = "simulate")
```

We do this many times until we can create a null distribution of our chi-squared statistics