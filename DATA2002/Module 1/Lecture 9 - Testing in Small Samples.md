## Learning Objectives
- Fisher's exact test
- Yates's Chi-Squared Test
- Permutation Testing

## Fisher's Exact Test
### Lady Tasting Tea
Given a cup of tea with milk, A lady claims she can determine whether tea or milk was poured into the cup first

A man named Fisher prepared 8 cups of with:
- 4 cups of tea with milk being added first
- 4 cups of tea with tea being added first

Fisher then randomly gave the cups to the women and asked her to identify the 4 cups where tea was added first. To determine if she has this skill we need to record:
- Which cups had tea or milk added first(**Truth**)
- Which cups the lady claimed had tea or milk added first (**Predicted**)
Fisher's experiment were left with two categorical variables and with the hypothesis: "**Are the predictions independent of the truth?**"

$$
\mbox{Truth = {Milk, Tea, Tea, Milk, Tea, Tea, Milk, Milk}}
$$
$$
\mbox{Prediction = {Milk, Tea, Tea, Milk, Tea, Tea, Milk, Milk}}
$$
Lets try and do a chi-square test of independence using R
```
truth = c("milk", "tea", "tea", "milk", "tea", "tea", "milk", "milk")
predicted = c("milk", "tea", "tea", "milk", "tea", "tea", "milk", "milk")
tea_mat = table(truth, predicted)

chisq.test(tea_mat, correct = FALSE)
## Warning: Approximation may be incorrect
## X-squared = 8, df = 1, p-value = 0.004678
```
### Fisher's Exact Test
The $\chi^2$ approximation is only reasonable when $n$ is reasonably large, i.e. the expected cell counts is $\geq 5$ . It is important to use exact tests to calculate the exact p-value for the test statistics and the key assumption is that the observations are independent

Note that this test can be used for both $2 \times 2$ tables and general contingency tables

### Formulating the null hypothesis
Let $\theta$ be the odds ratio and recall that if $A$ or $B$ are independent then the odds ratio will equal to 1. So when using the Fisher's exact test can be thought of as testing:
$$
H_0: 0 = 1
$$
against the alternative:
$$
H_1: \theta > 1 \space or H_1: \theta < 1 \space or \space H_1: 0 \neq 1
$$
The **test statistics** is $Y_{11}$ and the **observed test statistic** is $y_{11}$

### Aside: Hypergeometric Distribution
The hypergeometric distribution relates to sampling without replacement from a finite population

The conditions characterizing the hypergeometric distribution:
- Result of each draw pf the population can be classified into two mutually exclusive categories
- Probability of "success" changes on each draw, as each draw decreases the population

|         | Drawn  | Nor Drawn | Total |
| ------- | ------ | --------- | ----- |
| Success | $y$ | $y_{1\bullet} - y$    |     $y_{1\bullet}$  |
| Failure | $y_{\bullet 1} - y$ | $n+y-y_{\bullet 1} - y_{1\bullet}$}    |  $n-y_{1\bullet}$     |
| Total        | $y_{\bullet 1}$       | $n-y_{\bullet 1}$           |  $n$     |
$$
\small{P(Y=y)=\frac{{\displaystyle\binom{y_{1 \bullet}}{y}}{\displaystyle\binom{n-y_{1 \bullet}}{y_{\bullet 1}-y}}}{\displaystyle\binom{n}{y_{\bullet 1}}}},
$$
- $n$ - population size
- $y_{1\bullet}$ - number of success in the population
- $y_{\bullet 1}$ - number of samples
- $y$ - number of observed successes
## Lady Tasting Tea
$$
\scriptsize{
\begin{array}{l|cc|c}
& \mbox{Predicted} & \\
\mbox{Truth} & \mbox{Milk} & \mbox{Tea} &  \\ \hline
\mbox{Milk} & 4 & 0 & y_{1\bullet} = 4  \\
\mbox{Tea}  & 0 & 4 & y_{2\bullet} = 4 \\ \hline
& y_{\bullet 1} = 4 & y_{\bullet 2}=4 & y_{\bullet\bullet} = n = 8 \\ \hline
\end{array}
}
$$
For the Fisher's exact test we:
1. Consider all possible permutations of the $2 \times 2$ contingency table with the same marginal totals
	- $y_{i\bullet} = y_{\bullet j} = 4$
2. Calculate how many of these were equal to or "more extreme" than what we observed
### What does "more extreme" mean?
Let is define a test statistic:
$$
T = {Number \ of \ cups \ of \ tea \ before \ milk \ that \ she \ got \ correct}
$$
This test statistic has 5 outcomes: {0,1,2,3,4}
Given that there are 8 cups of tea, there are $8\choose4$ ways that we could predict which cups had tea added before milk
We can look at all 70 permutations of prediction vs truth and calculate how often we see a test statistic of 0, 1, 2, 3, or 4
$$
\scriptsize{
\begin{array}{l|ccccc|c}
t_i & 0 & 1 & 2 & 3 & 4 &  \\ \hline
f_i & \binom{4}{0}\binom{4}{4} = 1 & \binom{4}{1}\binom{4}{3} = 16 &  \binom{4}{2}\binom{4}{2} =36 &  \binom{4}{3}\binom{4}{1} = 16 &  \binom{4}{4}\binom{4}{0} = 1 & 70 \\
p_i & \frac{1}{70} & \frac{16}{70} & \frac{36}{70} &\frac{16}{70} &\frac{1}{70} & 1 \\ \hline
\end{array}
}
$$
$P(T=4) = \frac{1}{70} = 0.014$

## Another Example: Cancer of the Larynx
|                   | Cancer Controlled   | Cancer not Controlled               | Total            |
| ----------------- | ------------------- | ----------------------------------- | ---------------- |
| Surgery           | 21                 | 2                  | 23   |
| Radiation Therapy | 15 | 3 | 18 |
| Total             | 36                  | 5                                   | 41               |
|                   |                     |                                     |                  |
Suppose that we wish to test $H_0:\theta = 1$ (both treatments are equally effective) against $H_1:\theta > 1$

Because the expected cell counts will be less than 5, we need to use Fischer's test and to do that we first need to enumerate all tables which are **as extreme** or **more extreme** than the observed table

Let's have our test statistic be the number of surgery cases where cancer is controlled:

1.

|Cancer Controlled|Cancer not Controlled|Total|
|---|---|---|
|Surgery|22|1|23|
|Radiation Therapy|14|2|18|
|Total|36|5|41|

### Drawbacks
- Calculation of the p-value requires conditioning on row and column margins to be fixed
- Computationally difficult for large samples
- It can be generalized to $r \times c$ tables but difficult to compute
	- Generally requires use of Monte Carlo
## Yates's Chi-Squared Test
Yates modified the standard chi-squared test with a continuity correction as it is usually more accurate when counts in each cell are small. For the case for $2\times2$ tables is:
$$
T = \sum_{i=1}^2\sum_{j=1}^2 \frac{(|Y_{ij} - e_{ij}| - 0.5)^2}{e_{ij}}
$$
In general, if we have an *integer-valued* random variable $X$ which we would like to approximate with a continuous random variable $Y$ then:
$$
P(X\le x) \approx P(Y\le x+0.5) \quad \text{ and } \quad P(X\ge x) \approx P(Y \ge x-0.5).
$$

![[Pasted image 20230816170608.png]]
![[Pasted image 20230816170620.png]]

## Permutation Testing

Galton's study (1892) marked one of the first formal statistical examinations of association for contingency tables. His work involves determining the association of fingerprint characteristics of 105 fraternal male twins 
```
galton.dat <- matrix(c(5, 4, 1, 12, 42, 14, 2, 15, 10), 3, 3)
rownames(galton.dat) = c("Arches-B", "Loops-B", "Whorls-B")
colnames(galton.dat) = c("Arches-A", "Loops-A", "Whorls-A")
chisq.test(galton.dat)
## X-squared = 11.17, df = 4, p-value = 0.02472
```
### Monte Carlo Simulation

The **Monte Carlo Simulation Procedure** is as follows:

1.  we should analyze the sample as we would normally do in a hypothesis test up to the calculation of the test statistic.

2. From the original sample being analyzed, resample it LOTS of times
3. We can compile all the test statistics that we computed from the resampling and then find the probability of the our observed test statistic is greater than or equal to the distribution of test statistic

Note that the p-value is the area calculated by determining the proportion of the resampled test statistics as or more extreme than the observed test statistic. 

Also note that **No assumptions** are made about the underlying distribution of the population
```
galton.dat <- matrix(c(5, 4, 1, 12, 42, 14, 2, 15, 10), 3, 3)
rownames(galton.dat) = c("Arches-B", "Loops-B", "Whorls-B")
colnames(galton.dat) = c("Arches-A", "Loops-A", "Whorls-A")
chisq.test(galton.dat)
## X-squared = 11.17, df = 4, p-value = 0.02472

row_totals = rowSums(galton.dat)
col_totals = colSums(galton.dat)
B = 10000
set.seed(123)
x_list = r2dtable(n = B, 
                  r = row_totals,
                  c = col_totals)


rnd.chisq = numeric(B) # initialise an empty vector
for (i in 1:B){ # loop over B iterations
  # each time save the test statistic
  rnd.chisq[i] = chisq.test(x_list[[i]])$statistic
}
# what proportion of times did we observe a test statistic
# as or more extreme than what we observed?
sum(rnd.chisq >= 11.1699)/B

# par(cex = 1.8)
hist(rnd.chisq)
abline(v = 11.1699, col = "purple", lwd = 2)
axis(1, 11.1699, col.axis = "purple")
```

Instead of having to simulate and do the tedious stuff we can make this easier by:
```
chisq.test(galton.dat, simulate.p.value = TRUE, B = 10000)
```
