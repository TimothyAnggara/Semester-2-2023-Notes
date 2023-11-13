## What happens when Assumptions fail?
### Assumptions for the ANOVA
- Each sample is from a **normal population**
- All **population variances are equal**
There are many ways these assumption may be violated:
- Normality ok but equal variance is not
- Equal variance is ok but the normality is not
Some tools that may remedy these issues:
- Simulation
- Resampling with conditioning
## Relaxing Equal Variance Assumption
### Wolf River Data
A study was done in the Wolf River in Tennessee where it was near a dump site for chemicals. It was believed that the contamination of these chemicals were not constant across different depths
![[Pasted image 20231009134155.png]]
#### Checking Assumptions
```
wolf_long = wolf |> 
  pivot_longer(cols = Aldrin:HCB, names_to = "chemical", 
         values_to = "concentration") 
ggplot(wolf_long, aes(x = Depth, y = concentration)) + 
  geom_boxplot() + facet_wrap(~chemical) 
```
![[Pasted image 20230920160931.png]]
```
ggplot(wolf_long, aes(sample = concentration)) + 
  geom_qq() + geom_qq_line() + facet_grid(chemical ~ Depth)
```
![[Pasted image 20230920160947.png]]
### Assumptions
From the graph shown above normality is probably okay because the points lie reasonably close to the QQ line but the boxplots may suggest a different spread in each group
#### Outliers
If we try and plot a scatterplot for each of the data point we can see something interesting:
```
ggplot(wolf) + aes(x = HCB, y = Aldrin, shape = Depth, colour = Depth) + 
  geom_point(size = 5)
```
![[Pasted image 20230920161352.png]]
There are 3 possible outliers in our dataset which is the one triangle and the two circles. Although it may be so clear in the scatter plot, if we calculate the differences and plot them in a box plot, the output will reveal 3 outliers
```
wolf |> mutate(
  diff = HCB - Aldrin
) |> 
  ggplot() + aes(x = "", y = diff) + 
  geom_boxplot(outlier.size = 5) + 
  coord_flip()
```
![[Pasted image 20230920161508.png]]
### Relaxing the common variance assumption: All pairwise comparisons
A simply way to relax the common variance assumption is to just use a different $t$-test, in specific the Welch $t$-test and apply a Bonferroni correction
- Recall that the Welch test only assumes each sample is iid
#### Welch Test Pairwise Comparisons
```
t.test(wolf$Aldrin[wolf$Depth=="Middepth"],
       wolf$Aldrin[wolf$Depth=="Surface"])$p.value
# 0.06

t.test(wolf$Aldrin[wolf$Depth=="Middepth"],
       wolf$Aldrin[wolf$Depth=="Bottom"])$p.value
# 0.12

t.test(wolf$Aldrin[wolf$Depth=="Surface"],
       wolf$Aldrin[wolf$Depth=="Bottom"])$p.value
# 0.005
```
We have not done the Bonferroni correction in the test above so in order to do that we will need to perform it by hand and it is quite simple.
- Because we are doing 3 pairwise comparisons simultaneously, we will multiply the "unadjusted" p-values by 3 to get the "adjusted" p-values
```
3 * t.test(wolf$Aldrin[wolf$Depth=="Surface"],
           wolf$Aldrin[wolf$Depth=="Bottom"])$p.value
# 0.016
```
To do these much more easily in R, we can simply use the function `pairwise.t.test()`
```
pairwise.t.test(wolf$Aldrin, wolf$Depth, 
                p.adjust.method = "none", pool.sd = FALSE)
```
![[Pasted image 20230920162523.png]]
```
pairwise.t.test(wolf$Aldrin, wolf$Depth,
                p.adjust.method = "bonferroni", pool.sd = FALSE)
```
![[Pasted image 20230920162538.png]]
### Simultaneous Confidence Interval
To obtain a set of 3 simultaneous Bonferroni-style 95% confidence intervals, we divide the significance level by 3 and compute the new interval
- $(1 - (0.05/3)) \times 100 =98.\dot3\%$ Intervals
## Relaxing Normality Assumption
Instead of the assumption for saying that all observations come from the same normal distribution we may be able to say that all observations come from the same distribution
### Powerful Tool of Conditioning
Conditioning is a common tool that we used in testing on a "ancillary statistic"
- Ancillary statistic - a statistic that does not tell us anything useful
An example that we have done in on the **sign test**
- We condition on the number, $N$, of non-zeros (i.e. no ties)
### Conditioning on the Combined Sample
If we combine all the groups into one combined sample then the remaining "data" tells us nothing about the differences between groups
- The combined sample is an Ancillary Statistic
Once we condition on the combined sample, what is left is the "randomness" of the allocation of observation to groups
- Note that under the null hypothesis of "no differences between groups" **all possible allocations are equally likely**
#### Enumerating All possible Allocations: Exact p-values
Theoretically, we could compute an exact conditional p-value for any "sensible" statistic under this particular hypothesis with the formula:
$$
\frac{N!}{n_1!n_2!\ldots n_g!}
$$
Since each value is equally likely under the null hypothesis, we can use this to compute a p-value but with the factorials, it would take an incredibly long time to compute thus not practical
- What we should do is a resampling method and propose that we take 5000 possible permutations and observe how extreme our observe test statistic is
$$
\begin{align*}
P(T\geq t_0 ~|~ \text{combined sample}) = \frac{\text{no. allocations with }T\geq t_0}{\text{total no. allocations}}\,.
\end{align*}
$$
- What we did above is a procedure generally known as the **permutation test**
### Rugby Analysis
Rules for International Rugby was changed to make the game more "continuous" where the passages of "play" are longer between stoppages
- Lengths of time of passage of play were recorded in 10 games
	- First 5 under the old rules
	- Last 5 under the new rules
![[Pasted image 20230920171246.png]]
#### Permutation Test using ANOVA (F-test) Statistic
```
rugby_anova = aov(Time ~ factor(Game), data = rugby)
anova(rugby_anova)
```
![[Pasted image 20230920172414.png]]
- This loops takes a sample of size $B$ from all possible permutations and computes the value of the $F$-statistic
```
set.seed(1)
B = 2000
f_stat = vector(mode = "numeric", length = B)
for (i in 1:B){
  permuted_anova = aov(sample(rugby$Time)  ~ factor(rugby$Game))
  f_stat[i] = broom::tidy(permuted_anova)$statistic[1]
}
```
- Now that we have our permutation and our observed $F$-statistic, lets compute the p-value
```
mean(f_stat >= t_0)
# 0
```
## Using Ranks

### Kruskal-Wallis Test
A test which replaces each observation by its "global" ranka nd then computing the $F$-ratio as usual on the ranks
- The p-value is obtained using a permutation test approach or a "large-sample" $\chi^2$ approximation can also be used
- We can also just use the ANOVA test on the ranks rather than doing the Kruskal-Wallis Test

