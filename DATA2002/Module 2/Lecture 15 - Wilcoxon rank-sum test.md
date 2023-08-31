When to use Wilcoxon Rank-Sum Test

## Wilcoxon Rank-sum test
### The Wilcoxon Rank-Sum test
It is a non-parametric test used to compare two independent samples
- It relaxes the normality assumption and symmetry

In the Wilcoxon Rank-Sum test, we assume that the samples, $X_1, X_2, ..., X_n$ and $Y_1, Y_2, ...,Y_n$ are taken from two distinct populations that follow the same kind of distribution but differ in location
![[Pasted image 20230830224002.png]]
That is, $\mu_y = \mu_x + \theta$ where $\mu_y$ is the population mean of $Y, \mu_x$ is the population mean of $X$ and $\theta$ is a **location shift** parameter
### How the Test works
Let $R_1, R_2, ..., R_N$ with $N = n_x + n_y$ be the ranks of combined sample: $X_1, X_2, ..., X_{n_x}, Y_1, Y_2,...,Y_{n_y}$
- For one sample **Wilcoxon Signed-Rank** test, the ranks are summed over positive side of the differences
- For two sample **Wilcoxon rank-sum** test, the ranks are summed over one of the sample
- $W = R_1 + R_2 + ... + R_{n_x}$
If $H_0$ is true, then $W$ should be close to its expected value:
$$
\operatorname{E}(W) = \text{Proportion}\times\text{Total rank sum} = \frac{n_x}{N} \times \frac{N(N+1)}{2} = \frac{n_x(N+1)}{2}.
$$
If $W$ is small (large), we expect $\mu_x < \mu_y$ ($\mu_x > \mu_y$)
![[Pasted image 20230830225309.png]]
### Calculate p-value: no ties on the data
The exact p-value $P(W \leq w)$  is given in `R` by
`pwilcox(w-minw, m = nx, n = ny`
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
```
### Normal Approximation
- We can use a normal approximation to the distribution of test statistic:

### Latent Heat of Fusion Example
```
A = c()
```