When to use Wilcoxon Rank-Sum Test

## Wilcoxon Rank-sum test
### The Wilcoxon Rank-Sum test
It is a non-parametric test used to compare two independent samples
- It relaxes the normality assumption and symmetry

(How can we test if the two samples have the same distribution)
In the Wilcoxon Rank-Sum test, we assume that the samples, $X_1, X_2, ..., X_n$ and $Y_1, Y_2, ...,Y_n$ are taken from two distinct populations that follow the same kind of distribution but differ in location
![[Pasted image 20230830224002.png]]
That is, $\mu_y = \mu_x + \theta$ where $\mu_y$ is the population mean of $Y, \mu_x$ is the population mean of $X$ and $\theta$ is a **location shift** parameter
### How the Test works
Let $R_1, R_2, ..., R_N$ with $N = n_x + n_y$ be the ranks of combined sample: $X_1, X_2, ..., X_{n_x}, Y_1, Y_2,...,Y_{n_y}$

Simply put, we are going to pool together both samples into one big sample and rank them accordingly. Then we will sum the ranks associated with population X and the ranks associated with population Y

If the means of the two population were close, then we would expect the sum of the ranks to be not so different
- For one sample **Wilcoxon Signed-Rank** test, the ranks are summed over positive side of the differences
- For two sample **Wilcoxon rank-sum** test, the ranks are summed over one of the sample
- $W = R_1 + R_2 + ... + R_{n_x}$
	- The sum of the ranks from population X
If $H_0$ is true, then $W$ should be close to its expected value:
$$
\operatorname{E}(W) = \text{Proportion}\times\text{Total rank sum} = \frac{n_x}{N} \times \frac{N(N+1)}{2} = \frac{n_x(N+1)}{2}.
$$
If $W$ is small (large), we expect $\mu_x < \mu_y$ ($\mu_x > \mu_y$)
![[Pasted image 20230830225309.png]]
### Calculate p-value: no ties on the data
The exact p-value $P(W \leq w)$  is given in `R` by
`pwilcox(w-minw, m = nx, n = ny)` 
- (What is m and n)?
where $\min(W) = \underbrace{1+2+\ldots+n_x}_{n_x}=\dfrac{n_x(n_x+1)}{2}$
The distribution that `pwilcox()` uses is for the distribution of $W - min(W)$ and it starts at 0

### Yield Example
Let's say that in an experiment two methods of measurement gave two different results and we want to test if the data present has sufficient evidence to indicate a difference in the methods A and B
```
A = c(32, 29, 35, 28)
B = c(27, 31, 26, 25, 30)
dat = data.frame(
  yield = c(A,B),
  method = c(rep("A", length(A)),
             rep("B", length(B)))
)
dat = dat %>% mutate (rank = rank(yield))

w_A = dat %>% 
  filter(method == "A") %>% 
  pull(rank) %>% 
  sum()
```
![[Pasted image 20230830231155.png]]
```
sum_dat = dat %>%
	group_by(method) %>%
	summarise(n = n(),
		w = sum(rank))
sum_dat
# A tibble: 2 × 3
method n w 
<chr> <int> <dbl> 
1 A 4 26 
2 B 5 19

# This is pulling the sample size of A, B and ??
n_A = sum_dat |> 
  filter(method == "A") |> 
  pull(n)
n_B = sum_dat |> 
  filter(method == "B") |>
  pull(n)
# using the sums of the A sample
w_A = sum_dat |> 
  filter(method == "A") |> 
  pull(w)

# Calculates the expected counts
ew_A = n_A * (n_A + n_B + 1)/2 
# Calculates the smallest possible sum of ranks for the sample (Slide 10)
minw_A = n_A * (n_A + 1)/2 
```
Now that we've calculated our ranks and also gathered the sum of the ranks from population A, it is time to find the p-value using `pwilcox()`
```
2 * pwilcox(w_A - minw_A - 1, n_A, n_B, lower.tail = FALSE)
```
To explain the code above, our test statistic is 26, `w_A = 26`, and we want to find the area of the distribution where it is $P(W \geq 26)$ and we are multiplying by two because its a two-sample test. 

The reason why we are substracting the minimum possible sum of the ranks from population A is because the `pwilcox()` function doesn't start at 0, but we want it to start at 0. We also minus it by one because when we are looking at `lower.tail = FALSE`, it'll look at $P(W > 26) = P(W \geq 27)$ but we are interested in $P(W \geq 26)$ hence we look at $P(W > 25) = P(W \geq 26)$ (NEEDS TO BE ASKED IN CONSULATAION) 
![[Pasted image 20230831214924.png]]

### What if we use the sums of the ranks of B?
Instead of looking for the upper tail, we are now looking at the lower tail
```
2 * pwilcox(w_B - minw_B, n_B, n_A)
```
We are subtracting and multiply by two like the reasons above but we no longer need to subtract by one because the function will return $P(W-min(W) \leq 19-15)$  

To do it simply, we can just use the `wilcox.test()` function
- `wilcox.test(A,B)`
### Normal Approximation
- We can use a normal approximation to the distribution of test statistic:
$$
T = \frac{W-\operatorname{E}(W)}{\sqrt {\operatorname{Var}(W)}} \sim  \mathcal{N}(0,1) \text{ approximately},
$$
Where $\operatorname{E}(W) = \dfrac{n_x(N+1)}{2}$ and $\operatorname{Var}(W) = \dfrac{n_x n_y}{N(N-1)}\left( \displaystyle\sum_{i=1}^N r_i^2-\dfrac{N(N+1)^2}{4} \right)$

With the normal approximation, we can use the normal distribution to find our p-values with the calculation:
- $\text{p-value} \approx P\left(Z\geq \frac{W-\operatorname{E}(W)}{\sqrt {\operatorname{Var}(W)}} \right) \qquad \mbox{for } H_1\colon\ \mu_x>\mu_y$
- $\text{p-value} \approx P \left(Z\leq \frac{W-\operatorname{E}(W)}{\sqrt {\operatorname{Var}(W)}} \right) \qquad \mbox{for } H_1\colon\ \mu_x<\mu_y$
- $\text{p-value} \approx 2 P \left(Z\geq\left| \frac{W-\operatorname{E}(W)}{\sqrt {\operatorname{Var}(W)}} \right| \right) \qquad \mbox{for } H_1\colon\ \mu_x \not=\mu_y.$
### Latent Heat of Fusion Example
A company presents data from two methods that were used to study the latent heat of fusion of ice.
- Method A uses a digital method
- Method B used a method of mixtures
The two methods were conducted with specimens cooled to -0.72°C
- Data represents change in total heat from -0.72°C to water at 0°C in calories per gram of mass
```
A = c(79.98, 80.04, 80.02, 80.04, 80.03, 80.03, 80.04, 
      79.97, 80.05, 80.03, 80.02, 80.00, 80.02)
B = c(80.02, 79.94, 79.98, 79.97, 79.97, 80.03, 79.95, 
      79.97)
heat = data.frame(
  energy = c(A,B),
  method = rep(c("A","B"), c(length(A), length(B))))
```
**The question**: Does the data support the hypothesis that the electrical method (A) gives larger results?
- One sample, greater

```
heat = heat |> 
  dplyr::mutate(r = rank(energy))
# or equivalently
# heat$rank = rank(heat$energy)
heat |> arrange(r) 
energy method r 
1 79.94 B 1.0 
2 79.95 B 2.0 
3 79.97 A 4.5
4 79.97 B 4.5 
5 79.97 B 4.5 
6 79.97 B 4.5 
7 79.98 A 7.5
8 79.98 B 7.5 
9 80.00 A 9.0 
10 80.02 A 11.5 
11 80.02 A 11.5 
12 80.02 A 11.5 
13 80.02 B 11.5 
14 80.03 A 15.5 
15 80.03 A 15.5
```
Note that in this example, because there are ties we have to use the normal approximation
![[Pasted image 20230831220918.png]]


### Final Comments
- Similar to Wilcox-sign rank test, this test is also robust
	- Not that heavily affectively with outliers
- When the assumptions of the two-sample $t$-test hold, the Wilcoxon is less powerful than than the $t$-test
- If one is primarily interested in differences in location between the two distributions, the Wilcoxon test has the disadvantage of also reacting to other differences between the distributions such as differences in shape. (Don't understand sorry)
- In practical situation where we are uneasy about the applicability of two-sample $t$-test, we use both the $t$-test and the Wilcoxon and feel happiest when they both give similar conclusions