## Contingency Tables

Let's investigate the relationship between the variables party and national arms.
- The first variable contains the political party affiliation:
	- Republican, Democrat, or Independent
- The second variable contains options whether they think they are spending:
	- too much, too little, or about right amount of money on national defense

Let's try and construct a plot to compare proportions using the position = "fill" option

```{r}
ggplot(gss2016, aes(x = party, fill = natarms)) + geom_bar(position = "fill")
```

The plot that we just created shows us just the proportions but what if we want to see the count?

```
ggplot(gss2016, aes(x = party, fill = natarms)) + geom_bar()
```

To get a better idea of the data, we can create a contingency table which shows the the count of each group in what variable
- Count of each democratic party for what opinion 

To crate a contingency table, you select the columns of interest and ten send them to the table function

```{r}
tab <- gss2016 %>% 
	select(natrams, party) %>%
	table()
```

Using a contingency table to analyze data is a bit awkward and it is best to tidy up the data into a dataframe first using tidy()

```{r}
tab %>% 
	tidy() %>%
	uncount(n)
```

## Chi-squared Test Statistic

A hypothesis test is a test where you assume a hypothesis and generate a dataset which you can use to calculate a relevant test statistic and compare that to the observed test statistic.

The Chi-squared statistic captures the distance between a contingency table and the table you would expect if the variables were independent of each other

$$
\sum{\frac{(O_i - E_i)^2}{E_i}}
$$

**Let's do an example:**

Lets say we want to find if there is a difference between political party and their opinion in military spending

```{r}
null <- data %>%
	specify(var1 ~ var2) %>% 
	hypothesize(null = "independence") %>% 
	generate(reps = 100, type = "permute") %>%
	calculate(stat = "Chisq")
```


## Alternate Method - The Chi-Squared Distribution

We can conduct a hypothesis test of independence using the chi-squared statistic and using the normal distribution

The chi-squared distribution can be used to approximate the null distribution when trying to find the chi-squared statistic

The shape of the chi-squared distribution is dependent on the degrees of freedom, which can be found using:

$$
df = (nrows - 1) \times (ncols - 1)
$$
