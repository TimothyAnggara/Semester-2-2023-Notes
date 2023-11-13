## Using Residuals To Check for Normality
### Critical Flicker Frequency
If a light is flickering at a high frequency, it appears to not be flickering it all. Hence there exist a "critical flicker frequency" where the flickering changes from "nondetectable" to "detectable". A study was done from 19 people into investigating the relationship between iris color and the critical frequency flicker.
```
path = "https://raw.githubusercontent.com/DATA2002/data/master/flicker.txt"
flicker = read_tsv(path)
glimpse(flicker)
```
![[Pasted image 20230918162333.png]]
#### Checking Assumptions
```
p1 = ggplot(flicker) + aes(x = Colour, y = Flicker) + geom_boxplot() + 
  labs(y = "Critical flicker frequency", x = "Eye colour")
p2 = ggplot(flicker) + aes(sample = Flicker) + 
  geom_qq_line() + geom_qq() + facet_wrap(~ Colour) +
  theme(axis.text.x = element_blank())
cowplot::plot_grid(p1, p2, ncol = 2, rel_widths = c(1.1,2),axis = "lrtb", align = "hv")
```
![[Pasted image 20230918162359.png]]
#### Checking For Normality With Residuals
Population model is $Y_{ij} = \mu_i + \varepsilon_{ij},$
- Where $\varepsilon_{ij}\sim \mathcal{N}(0,\sigma^2)$
Rather than looking at QQ plots for each sample, we can instead consider the ANOV residuals to check for normality
$$
r_{ij} = y_{ij} - \bar{y}_{i\bullet}.
$$
- If the ANOVA assumptions hold true, the residuals should be normally distributed
```
flicker_anova = aov(Flicker ~ Colour, 
                    data = flicker)
flicker_resid = flicker_anova$residuals
ggplot(data.frame(flicker_resid)) +
  aes(sample = flicker_resid) + 
  geom_qq_line() + geom_qq(size=3)
```
![[Pasted image 20230918162714.png]]
#### Summary Statistics
Let's generate some summary statistics which will be useful later
```
sum_stat = flicker |> group_by(Colour) |>
  summarise(n_i = n(),
            ybar_i = mean(Flicker),
            v_i = var(Flicker))
sum_stat
# A tibble: 3 × 4 
Colour n_i ybar_i v_i 
<chr> <int> <dbl> <dbl> 
1 Blue 6 28.2 2.33 
2 Brown 8 25.6 1.86 
3 Green 5 26.9 3.40
```
```
n_i = sum_stat |> pull(n_i)
n_i # 6 8 5

ybar_i = sum_stat |> pull(ybar_i)
ybar_i # 28.167 25.5875 26.92

v_i = sum_stat |> pull(v_i)
v_i # 2.33467 1.864 3.397
```

### ANOVA Results
```
summary(flicker_anova)
```
![[Pasted image 20230918163449.png]]
From the results, we observe a small p-value which suggest that we should reject the ANOVA null hypothesis and we can consider a "contrast" to check which means are different to the others
- In general, there may be more than one "contrast of interest"

## Multiple Comparisons: Simultaneous Confidence Interval
### All Pairwise Differences
When we observe that no single group is "special" or notable so that each pairwise difference is equally interesting, we can consider each pairwise difference as a contrast of interest
- In this case,
	- a $t$-statistic can be constructed for each pairwise difference
	- a $t$-based confidence interval can be constructed for each pairwise "population" difference

#### Individual 95% Confidence Intervals
- We can construct a 95% confidence interval for each pairwise comparison individually:
- The standard error for $\bar y_{i\bullet}-\bar y_{h\bullet}$ is $\hat{\sigma} \sqrt{\frac{1}{n_i}+\frac{1}{n_h}}$
	- $\bar y_{i\bullet}-\bar y_{h\bullet}$ - Difference in means / pairwise mean difference
- Confidence interval will look something like
$$
\bar y_{i\bullet}-\bar y_{h\bullet} \pm t^*(\hat{\sigma} \sqrt{\frac{1}{n_i}+\frac{1}{n_h}})
$$
```
N = length(flicker_resid)
g = 3
sig_sq_hat = sum(flicker_resid^2)/(N-g) # Mean square resiudal
sig_sq_hat $ 2.39438
```
```
# alternatively
# sig_sq_hat = sum((n_i - 1) * v_i)/sum(n_i - 1)
t_star = qt(.975, df = sum(n_i - 1))
t_star # 2.119905
```
#### Blue vs Brown
```se.Bl.Br = sqrt(sig_sq_hat * ((1/n_i[1]) + (1/n_i[2])))
(int.Bl.Br.95.indiv = ybar_i[1] - ybar_i[2] + c(-1,1) * t_star * se.Bl.Br)
# 0.808 4.35 
```
#### Blue vs Green
```
se.Bl.Gr = sqrt(sig_sq_hat * ((1/n_i[1]) + (1/n_i[3])))
(int.Bl.Gr.95.indiv = ybar_i[1] - ybar_i[3] + c(-1, 1) * t_star * se.Bl.Gr)
# -0.740 3.23
```
#### Green vs Brown
```
se.Gr.Br = sqrt(sig_sq_hat*((1/n_i[2]) + (1/n_i[3])))
(int.Gr.Br.95.indiv = ybar_i[2] - ybar_i[3] + c(-1, 1) * t_star * se.Gr.Br)
# -3.203 0.538
```

We retain the null hypothesis for Blue vs Green and Green vs Brown but we reject for Blue vs Brown because its confidence interval does not contain zero

### Summary of Individual Intervals
It appears that the only significant different pair is Blue and Brown but how can we be so sure that what we observed is not a false positive (Type I error)

Let us use what we lecture 19 to minimize this error

## Bonferroni Method
- Let $A_1, A_2$ and $A_3$ denote the events where each of the 3 intervals above cover the corresponding "true" value
- Under our normal-equal-variance model we have:
$$
P(A_1)=P(A_2)=P(A_3)=0.95\,.
$$
- However, what is: $P \left( A_1\cap A_2\cap A_3 \right)$?
- This is a bit hard but we can derive a lower bound by using the relation:
$$
\left( A_1\cap A_2\cap A_3 \right)^c=A_1^c\cup A_2^c\cup A_3^c\,.
$$
- Recall that $P(A\cup B)\leq P(A)+P(B)$, hence:
$$
\begin{align*}
1- P \left( A_1\cap A_2\cap A_3 \right) = P\left\{ \left( A_1\cap A_2\cap A_3 \right)^c\right\}&= P\left(A_1^c\cup A_2^c\cup A_3^c\right) \\
&\leq P(A_1^c)+P(A_2^c)+P(A_3^c)\\
&= 0.05+0.05+0.05=0.15.
\end{align*}
$$
- Thus, $P(A_1\cap A_2\cap A_3)\geq 0.85$

I ripped that all from the slides but basically what we want to do with the Bonferroni correction is that we take our original significance level and divide that by the number of test performed, 
- $0.05 / 3 = \frac{1}{60}$

### Make our Intervals a bit Wider
- So what we do is we make our intervals a bit wider
- We recalculate the significance level to be:
$$
1-(0.05)/3=59/60=0.98\dot3
$$
```
t_simul = qt(1 - (0.05)/6, df = sum(n_i - 1))
t_simul # 2.67
```
Once we have our new $t^*$,  we can compute our new confidence intervals

#### Blue vs Brown
```
(int.Bl.Br.95.simul = ybar_i[1] - ybar_i[2] + c(-1,1) * t_simul * se.Bl.Br)
# 0.345 4.813
```
#### Blue vs Green
```
(int.Bl.Gr.95.simul = ybar_i[1] - ybar_i[3] + c(-1,1) * t_simul * se.Bl.Gr)
# -1.257 3.751
```
#### Green vs Brown
```
(int.Gr.Br.95.simul = ybar_i[2] - ybar_i[3] + c(-1, 1) * t_simul * se.Gr.Br)
# -3.69 1.03
```
## Multiple Comparisons: Pairwise $t$-test
A general $t$-test for a contrast takes the form:
$$
t_0 = \frac{\sum_{i=1}^g c_i \bar{y}_{i\bullet}}{\hat{\sigma} \sqrt{\sum_{i=1}^g c_i^2/n_i}}
$$
and in our particular case it may look something like:
$$
t_0 = \frac{\bar{y}_{1\bullet} - \bar{y}_{2\bullet}}{\hat{\sigma} \sqrt{1/n_1 + 1/n_2}}
$$
#### Blue vs Brown
```
se.Bl.Br = sqrt(sig_sq_hat *
                  ((1/n_i[1]) + (1/n_i[2])))
t_stat.Bl.Br = (ybar_i[1]-ybar_i[2])/se.Bl.Br
2*(1-pt(abs(t_stat.Bl.Br), df = sum(n_i-1)))
# 0.007
```
#### Blue vs Green
```
t_stat.Bl.Gr=(ybar_i[1]-ybar_i[3])/se.Bl.Gr
2*(1-pt(abs(t_stat.Bl.Gr),df=sum(n_i-1)))
# 0.202
```
#### Brown vs Green
```
t_stat.Gr.Br=(ybar_i[2]-ybar_i[3])/se.Gr.Br
2*(1-pt(abs(t_stat.Gr.Br),df=sum(n_i-1)))
# 0.15
```
#### Use Emmeans
- No Adjustments
```
contrast(flicker_em, method = "pairwise", adjust = "none")
```
![[Pasted image 20230918211759.png]]
- With Bonferroni Adjustment
```
contrast(flicker_em, method = "pairwise", adjust = "bonferroni")
```
![[Pasted image 20230918211827.png]]

## Tukey's Method
Another way we can use to control the family error rate is the **Tukey's method** but it only works where are are interested in the pairwise differences and **equal number of observations in each group**

Tukey's method considers the exact multiplier needed for simultaneous confidence intervals for all pairwise comparisons when the sample sizes are equal

## Scheffe's Method
The math isn't important and the behind the scenes doesn't seem to be examinable so what we are effectively doing is that we are comparing each contrast $t$-statistic to the $\sqrt{(g-1)F}$ distribution and any $t$-statistic that exceeds the critical value is significant in the "simultaneous" sense

The advantage of this method is that we can use it for **data snooping**
- If we are not sure which comparisons you want to look at
