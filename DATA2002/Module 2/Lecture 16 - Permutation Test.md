## Lady Testing Tea
Recall that in the Lady Testing Tea experiment we used combinatorics to find the exact t-value hence the exact p-value. Another way we can do this is to find all the possible permutations and to find the proportion that we can see what we observed
```
truth = c("milk","tea","tea","milk","tea","tea","milk","milk")
permute_guess = permutations(truth)
permute_guess[92, ] # 92nd permutation 
# milk,tea,tea,milk, milk, milk, tea, tea
identical(truth, truth) # Truth
identical(permute_guess[92,], truth) # False
```

### Exact p-value
We can calculate the exact p-value by looking across all permutations which would be the same as we get using Fisher's exact test
```
B = nrow(permute_guess)
check_correct = vector("numeric", length = B)
for(i in 1:B) {
  check_correct[i] = identical(permute_guess[i,], truth)
}
c(sum(check_correct), mean(check_correct))
```
### Approximate P-value
Calculating all the possible permutations takes a lot of computing power so it is more feasible to just take a subset of all the possible permutation
```
set.seed(123)
truth = c("milk","tea","tea","milk","tea","tea","milk","milk")
B = 10000
result = vector(length = B) # initialise outside the loop
for(i in 1:B){
  guess = sample(truth, size = 8, replace = FALSE) # does the permutation
  result[i] = identical(guess, truth)
}
mean(result)
```

## Permutation Test: Two Independent Samples
### Plant Growth
Results from an experiment to compare yields obtained under a control and two different treatment conditions
```
# built into R, make it available
data("PlantGrowth") 
library(tidyverse)
PlantGrowth |> ggplot() +
  aes(y = weight, x = group, 
      colour = group) + 
  geom_boxplot(coef = 10) + 
  geom_jitter(width = 0.1, size = 5) + 
  theme(legend.position = "none") +
  labs(y = "Weight (g)", x = "Group")
```
![[Pasted image 20230904163439.png]]

In this example, we want to compare the **control group** to the **treatment 2 group**
```
dat = PlantGrowth |> filter(group %in% c("ctrl", "trt2"))
```

### What does our "Standard" Methods say?
With a two-sample $t$-test, we find that our test statistic is -2.134 and a p-value of 0.04: `t.test(weightt ~ group, data = dat, var.equal = TRUE)`

With a Wilcoxon rank-sum test we find that our p-value is 0.06 with a test statistic of 25: `wilcox.test(weight ~ group, data = dat)`
### Permutation Test
Permute the class labels and see what values we get for the $t$-statistic and see what values we get for the $t$-test statistic
```
B = 10000 # number of permuted samples we will consider
permuted_dat = dat # make a copy of the data
t_null = vector("numeric", B) # initialise outside loop
for(i in 1:B) {
  permuted_dat$group = sample(dat$group) # this does the permutation
  t_null[i] = t.test(weight ~ group, data = permuted_dat)$statistic
}
```
To find the p-value, we need to find the proportion of test statistics from randomly permuted data are more extreme than the test statistic we observed?
```
mean(abs(t_null) >= abs(tt$statistic)) # 0.05
```

Permutation test only assumes that the observations $X_1, X_2, ..., X_n$, and $Y_1,Y_2,...,Y_n$ are exchangeable under $H_0$ that is, swapping labels on observations keeps the data just as likely as the original
(???)
### Latent Heat of Fusion
A study was done to test two different methods of measurement of the latent heat of fusion of ice. Both method A (Digital) and method B (Method of mixtures) were conducted with specimens cooled to -0.72°C
```
A = c(79.98, 80.04, 80.02, 80.04, 80.03, 80.03, 80.04, 
      79.97, 80.05, 80.03, 80.02, 80.00, 80.02)
B = c(80.02, 79.94, 79.98, 79.97, 79.97, 80.03, 79.95, 
      79.97)
heat = data.frame(
  energy = c(A,B),
  method = rep(c("A","B"), c(length(A), length(B)))
)
tt = t.test(energy ~ method, data = heat, alternative = "greater")
tt ## t = 3.25, p-value = 0.00347
```
If we tried to do an exact permutation test on this data, it would take a ridiculously long time to do as $21! = 5.1*10^{19}$  so we have to do an approximate
```
B = 10000 # number of permuted samples we will consider
permuted_heat = heat # make a copy of the data
t_null = vector("numeric", B) # initialise outside loop
for(i in 1:B) {
  permuted_heat$method = sample(heat$method) # this does the permutation
  t_null[i] = t.test(energy ~ method, data = permuted_heat)$statistic
}
mean(t_null>=t0_original) #0.0036
```
(Why didn't we specify alternative = "greater" in t.test())


#### Outliers
Note that the permutation test, isn't very robust. If we were to encounter an outlier in our dataset, it would result in a very different p-value.

If we change a value from the B method from 80.02 to 80.2:
```
t.test(energy ~ method, data = heat1, alternative = "greater") 
# t = 0.637
# p = 0.2713
wilcox.test(energy ~ method, data = heat1, 
            alternative = "greater", correct = FALSE)
# t = 80.5
# p = 0.01859
```
Even if we did a permutation test using the Wilcoxon Rank-sum Statistic, we'll get a p-value much larger than what we see previously
```
t0_original = wilcox.test(energy ~ method, data = heat1)$statistic
set.seed(1234)
B = 10000
permuted_heat1 = heat1
t_null = vector("numeric", B)
for(i in 1:B){
  permuted_heat1$method = sample(heat1$method)
  t_null[i] = wilcox.test(energy ~ method, data = permuted_heat1)$statistic
}
mean(t_null >= t0_original) #0.0192
```

#### Robustly Standardized Difference in Medians

$$T = \frac{\widetilde{x} - \widetilde{y}}{\operatorname{MAD}(x) + \operatorname{MAD}(y)}$$
```
median_a = median(heat1$energy[heat1$method=="A"])
median_b = median(heat1$energy[heat1$method=="B"])
mad_a = mad(heat1$energy[heat1$method=="A"])
mad_b = mad(heat1$energy[heat1$method=="B"])
t0_original = (median_a - median_b)/(mad_a + mad_b)

B = 10000
t_null = vector("numeric", B)
for(i in 1:B){
  permuted_heat1$method = sample(heat1$method)
  median_a =  median(permuted_heat1$energy[permuted_heat1$method=="A"])
  median_b =  median(permuted_heat1$energy[permuted_heat1$method=="B"])
  mad_a = mad(permuted_heat1$energy[permuted_heat1$method=="A"])
  mad_b = mad(permuted_heat1$energy[permuted_heat1$method=="B"])
  t_null[i] = (median_a - median_b)/(mad_a + mad_b)
}
mean(t_null >= t0_original) # 0.0168
``` 
## Permutation Test: Paired Samples
For a paired test, we think about the differences, $d_i = x_i - y_i$
- For the Wilcoxon signed-rank test we had a test statistic:
$$
\sum_{i:\ d_i>0} r_i \times \operatorname{sign}(d_i)
$$
- We could also think of a statistic where we used the values of the differences,
$$
\sum_{i = 1}^n |d_i| \times \operatorname{sign}(d_i)
$$
- For a permutation test permute all possible sign$(d)i)$
### Smoking
Blood samples from 11 individuals before and after they smoked a cigarettes are used to measure aggregation of blood platelets
```
before = c(25, 25, 27, 44, 30, 67, 53, 53, 52, 60, 28)
after =  c(27, 29, 37, 36, 46, 82, 57, 80, 61, 59, 43)  
d = after - before
t.test(d) # t = 2.9065 | p = 0.01566
``` 
For a paired test, we want to permutate all possible signs of the differences and to do that:
```
sign_permute = permutations(c(-1,1), 11, replace = TRUE)
```
Thus to complete the permutation test for the two-sample t-test
```
(t0_original = mean(d)/sd(d)*sqrt(length(d)))
# Start of pertmutation Test
n = length(d)
B = nrow(sign_permute)
t_null = vector("numeric", B)

for(i in 1:nrow(sign_permute)){
  d_permute = d*sign_permute[i,]
  t_null[i] = mean(d_permute)/sd(d_permute)*sqrt(n)
}

mean(abs(t_null) >= abs(t0_original)) # P-value = 0.0157
```