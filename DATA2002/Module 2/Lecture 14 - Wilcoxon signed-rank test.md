## Learning Objectives
- Wilcoxon Signed-rank test
- Normal Approximation to the Wilcoxon signed-rank test statistic

## Wilcoxon Signed-Rank Test
The problem with the sign test is that we ignore a lot of information
- Inefficient use of data and having low power

How can we use more information than just the sign for data with symmetric but possible non-normal distribution?

### Ranks to the rescue
Many non-parametric tests are based not on the data but on their ranks. To find the ranks for a set of data:
- Arrange the data in ascending order
	- lowest to highest
- Assign a rank of 1 to the smallest, 2 to the second smallest, etc.
- For tied observations, assign each the average of the corresponding ranks
- ![[Pasted image 20230829230823.png]]

### Thinking about Magnitude
Under the symmetric distribution assumption with mean $\mu_0$ from $H_0$, half of the $d_i = x_i - \mu_0$ should be negative and half positive and the expected counts are both $n/2$
- Under the null hypothesis, the positive and negative $d_i$ should be similar in magnitude to occur with equal probability
- We rank the absolute values of $d_i$ in ascending order, the expected rank sums the negative and positive $d_i$, should be nearly equal

### Wilcoxon Signed-Rank Test
We must define some quantities:
- $D_i = X_i - \mu_0$ for $i = 1,2,...,n$
- $R_1,...,R_n$ be the ranks $|D_1|, |D_2|, ..., |D_n|$
- $W^+$ be the sum of the ranks $R_i$ corresponding to positive $D_i$
- $W^-$ be the sum of the ranks $R_i$ corresponding to negative $D_i$
- Let $W$ = $min(W^+, W^-)$

We should:
- reject $H_0: \mu = \mu_0$ in favor of $H_1: \mu > \mu_0$ if $w^+$ is large enough
- reject $H_0: \mu = \mu_0$ in favor of $H_1: \mu < \mu_0$ if $w^+$ is large small
- reject $H_0: \mu = \mu_0$ in favor of $H_1: \mu \neq \mu_0$ if $w = min(w^+,W^-)$ is small enough
![[Pasted image 20230829231658.png]]
### Calculation of p-value: no ties
The exact p-value of the Wilcoxon Signed-Rank Test is:
$$
P(W^+ \ge w^+) = P(W^+ \le n(n+1)/2-w^+)
$$
Note that when we use the r function `psignrank()` this is how it works, let say that we have a sample of size 5 and we observed $w^+ = 12$ 
```
# Instead of writing 
psignrank(12, 5, lower.tail = FALSE) P(W+ >= 13)
# We should write
psignrank(12 - 1, 5, lower.tail = FALSE) # P(W^+ >= 12)
```
Notice that:
$$
W^+ + W^- = 1+ 2 + \ldots + n = n(n+1) \frac{1}{2} \Rightarrow W^- = n(1+n) \frac{1}{2} - W^+
$$
Hence, under the null hypothesis, $E(W^+) = n(1+n)\frac{1}{4}$ and if there are **NO TIES** $Var(W^+) = n(n+1)(2n+1)/24$
### Weight Gain
```
y = c(85, 69, 81, 112, 77, 86)
x = c(83, 78, 70, 72, 67, 68)
d = y - x
d # 2 -9 11 40 10 18
```
Is there a weight **gain** in taking diet Y compared with diet X?
- Because of gain we are going to do a one-sided test with the difference being calculated by `y - x`
```
w_calc = data.frame(
  dif = d,
  absDif = abs(d),
  rankAbsDif = rank(abs(d)),
  signrank = sign(d)*rank(abs(d)) 
)
dif absDif rankAbsDif signrank 
1 2 2 1 1 
2 -9 9 2 -2 
3 11 11 4 4 
4 40 40 6 6 
5 10 10 3 3 
6 18 18 5 5

w_calc %>% 
  filter(signrank>0) %>% 
  summarise(sum(signrank)) %>% 
  pull()
  # 19
```
![[Pasted image 20230829232459.png]]
To do this test automatically we can simply just:
```
wilcox.test(d, alternative = "greater")
wilcox.test(y,x, alternative = "greater", paired = TRUE)
```

## Normal Approximation to the Wilcoxon Signed-Rank Test Statistic
### Normal Approximation
As $n$ increases as we conduct the Wilcoxon Test, it starts to look more like a normal distribution very quickly

For a **large enough** $n$, we can use a normal distribution to approximate the distribution of the Wilcoxon sign rank test statistic
$$
W^+ \sim \mathcal{N}\left( \frac{n(n+1)}{4}, \frac{n(n+1)(2n+1)}{24}\right),\quad \text{approximately}.
$$
Hence a large sample test statistic is:
$$
T = \frac{W^+ - \operatorname{E}(W^+)}{\sqrt{\operatorname{Var}(W^+)}} \sim \mathcal{N}(0,1),
$$

### Bus Waiting Times
The following data are waiting times for the 370 bus in minutes for 10 randomly selected passengers
```
bus = c(25, 19, 9, 27, 8, 7, 26, 12, 29, 20)
```
The bus authority claims a typical wait time of 15 minutes, does the data suggest a different typical wait time?
- Let us conduct a test with $H_0: \mu = 15$
![[Pasted image 20230829234233.png]]
####  Test Statistic
![[Pasted image 20230829234431.png]]
$$
w_+ = 7 + 2 + 8 + 9 + 10 + 3 = 39
$$
$$
w_- = |-4 + -5 + -6 + -1| = 16
$$
Test statistic: $w$ = min$(w^+, w^-) = 16$
If $H_0$ is true, $W^+$ comes from a symmetric distribution with mean:
$$
\operatorname{E}(W^+) = \dfrac{n(n+1)}{4} = \dfrac{10 \times 11}{4} = 27.5
$$
And variance,
$$
\operatorname{Var}(W^+) = \dfrac{n(n+1)(2n+1)}{24}=96.25
$$
Hence the observed test statistic being:
$$
t_0 =\frac{w - \operatorname{E}(W^+)}{\sqrt{\operatorname{Var}(W^+)}} = \frac{16-27.5}{\sqrt{96.25}} = -1.172
$$
- Note that from here we can conclude whether or not we would reject or retain the null hypothesis.
	- Because we can approximating the normal distribution, we know that the 95% distribution is at around 1.96 standard deviation away and our test statistic in within the 1.96 so we can retain the null hypothesis

Let's calculate the p-value anyway as a sanity check:
$$
\small{
\begin{align*}
2P(W^+ \leq 16) & \approx 2P\left(Z\leq\frac{16 - \operatorname{E}(W^+)}{\sqrt{\operatorname{Var}(W^+)}}\right)\\
& = 2P\left(Z\leq \frac{16-27.5}{\sqrt{96.25}} \right)\\
& = 2P(Z \leq -1.172) \\
& = \texttt{2*pnorm(-1.172)}\\
& = 0.241
\end{align*}
}
$$
```
# Manually
2 * psignrank(16, 10)
# Automatically
wilcox.test(bus-15)
```
### Normal Approximation with ties
We can still approximate $W^+$ by a normal distribution and the p-value is approximately given by,
$$
\begin{align*}
\text{p-value} & \approx  P\left(Z \geq \textstyle\frac{w^+ - \operatorname{E}(W^+)}{\sqrt{\operatorname{Var}(W^+)}} \right)
\quad \mbox{for } H_1\colon\  \mu>\mu_0, \\
\text{p-value} & \approx  P\left(Z \leq  \textstyle\frac{w^+ - \operatorname{E}(W^+)}{\sqrt{\operatorname{Var}(W^+)}} \right)
\quad \mbox{for } H_1\colon\  \mu<\mu_0, \\
\text{p-value} & \approx  2P\left(Z \geq  \textstyle\left|\frac{w^+ -\operatorname{E}(W^+)}{\sqrt{\operatorname{Var}(W^+)}} \right| \right)
\quad \mbox{for } H_1\colon\  \mu\not =\mu_0,
\end{align*}
$$
where in general, $\operatorname{E}(W^+) = \frac{1}{2}\sum_{i: \, d_i\not= 0}r_i$ and $\operatorname{Var}(W^+) = \frac{1}{4} \sum_{i:\, d_i\not= 0}r_i^2$

### Smoking
Blood samples from 11 individuals before and after they smoked a cigarette are used to measure aggregation of blood platelets
```
before = c(25, 25, 27, 44, 30, 67, 53, 53, 52, 60, 28)
after =  c(27, 29, 37, 36, 46, 82, 57, 80, 61, 59, 43)
df = data.frame(before, after,
                difference = after-before)
```
Is the aggregation of blood platelets affected by smoking?
```
df = df %>% dplyr::mutate(absDif = abs(difference),
                          rankAbsDif = rank(absDif),
                          srank = sign(difference)*rank(abs(difference)))
df
```
before after difference absDif rankAbsDif srank 
1 25 27 2 2 2.0 2.0 
2 25 29 4 4 3.5 3.5 
3 27 37 10 10 7.0 7.0 
4 44 36 -8 8 5.0 -5.0 
5 30 46 16 16 10.0 10.0 
6 67 82 15 15 8.5 8.5 
7 53 57 4 4 3.5 3.5 
8 53 80 27 27 11.0 11.0 
9 52 61 9 9 6.0 6.0 
10 60 59 -1 1 1.0 -1.0 
11 28 43 15 15 8.5 8.5
```
(w_p = sum(df$srank[df$srank > 0])) # 60
(w_m = sum(-df$srank[df$srank < 0])) # 6
(w = min(w_p, w_m)) # 6
```
![[Pasted image 20230830000406.png]]
### Final notes
Since we are assuming that the distribution is symmetric, the hypothesis can also be stated in terms of the median

The p-value from a Wilcoxon signed-rank test will typically be smaller than the p-value of a sign test on the same data. Using the information in the ranks, the test becomes much more **powerful** in detecting differences from $\mu_0$, and almost as powerful as the one sample $t$-test.