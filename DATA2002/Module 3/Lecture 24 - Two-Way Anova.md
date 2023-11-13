 ## Two-way ANOVA: Adjusting for "Blocks"
### Electrode Testing
A study done on skin resistance where 5 types of electrodes were attached to each of 16 subjects and the resistance was measured
- Do all 5 electrodes perform similarly?
We want to figure out the differences between the electrode types and/or between the subjects
- We are interested in test whether the means across the 5 different electrodes and the same but we should consider that there might be something going on with each subject that we necessarily don't want to study
![[Pasted image 20230920220549.png]]
### Box Plots
- Lets look at the boxplots for each electrode type ignoring the **subject**
```
p1 = ggplot(resist_long) + 
  aes(x = electrode, y = resistance) + 
  geom_boxplot() + coord_flip()
p1
```
![[Pasted image 20230920221056.png]]
- We can see that the boxplots look a bit skewed to the right. We can see that the medians are much closer to the lower quartile in E5, E4, and E1 with the lower whisker being much shorter than the upper whisker
- Let's try and see what the boxplots may look like if the scale it logarithmically
```
p1 + scale_y_log10() 
```
![[Pasted image 20230920221109.png]]
- They don't look perfect but they are much better and more symmetric now

Lets define a new variable $y$ in our data frame which is the log of the resistance measurement
```
resist_long = resist_long |> 
  mutate(
    y = log(resist_long$resistance)
  )
# alternatively
# resist_long$y = log(resist_long$resistance)
```
Now let's try and do a one-way ANOVA ignoring the **subjects**
```
fit1 = aov(y ~ electrode, data = resist_long)
summary(fit1)
```
![[Pasted image 20230920221609.png]]
- The p-value is greater than the significance levels which indicates that there is evidence to retain the null hypothesis
### Adjusting for Subject
We can add Subject as an extra factor variable in our formula to indicate that it should be used to help 'explain' `y` and to do this we write `y ~ Subject + electrode` in our `aov()`
```
fit2 = aov(y ~ Subject + electrode, data = resist_long)
summary(fit2)
```
![[Pasted image 20230920221925.png]]

How is it after we adjusted for Subject, our p-values becomes much smaller compared to the scenario where we didn't.
- When looking at both tables, we can see that the `Residual Sum of Squares` is much smaller after adjusting for Subject because we deconstructed `fit1`'s Residual sum of squares into `fit2`'s
	- Subject sum of squares (33.27, on **15** df)
	- Residual sum of squares (30.21, on **60** df)

Is the p-value for the subject significant?
- The null hypothesis if we were to consider states: Is the mean resistance among the subjects the same?
- The scientists probably don't really care about that because they already knew about it before hand.
	- Every person is different so it makes sense that the means would be different
### Changing Parameters
- For **ordinary one-way ANOVA**, we have written the model as: for $i = 1,...,g$, $j = 1,...,n_i$ 
$$
Y_{ij}\sim {\mathcal N}(\mu_i,\sigma^2)\,.
$$
- This leads to the model: for $i=1,\ldots,g$, $j = 1,..,n_i$
$$
Y_{ij}=\mu_i + \varepsilon_{ij}
$$
- where the $\epsilon_{ij}$ are iid ${\mathcal N}(0,\sigma^2)$ 
	- Again, there are g unknown mean-value parameters, and 1 unknown variance parameters
- Another way to write the model is that
$$
\mu_i = \mu + \alpha_i
$$
	- An overall mean $\mu$
	- An adjustment $\alpha_i$ for the $i$-th level of the treatment
- This leads to the model:
$$
Y_{ij} = \mu + \alpha_i + \varepsilon_{ij}.
$$
- Note that doing this, there are now $g+1$ "mean" parameters and we have "created" another parameter but also not really
	- There is a constraint of the $\alpha$ that we just created as they must sum to zero because they are deviations from the mean
		- Hence not really a new parameter
### Estimating the new "parameters" ($\alpha$)
- We could estimate these alphas and using the formula below:
$$
\hat\alpha_i = \bar y_{i\bullet}-\bar y_{\bullet\bullet}\,.
$$
### The two-way ANOVA Model
- The model we will fit our electrode data is the following:
$$
Y_{ij} = \mu + \alpha_i + \beta_j + \varepsilon_{ij}
$$
$$
\begin{aligned}
\mu       & = \text{overall mean}\\
\alpha_i  & = \text{ adjustment for electrode type }i\text{ for }i=1,2,\ldots,g \\
\beta_j   & = \text{ adjustment for subject }j\text{ for }j=1,2,\ldots,n
\end{aligned}
$$
### Estimating Parameters
- We can estimate our alpha for the electrode type $i$ using the formula that we saw before:
$$
\bar y_{i\bullet}-\bar y_{\bullet\bullet}\ .
$$
- We can estimate our beta for the subject $j$  using this formula that was used in previous lectures
$$
\bar y_{\bullet j}-\bar y_{\bullet\bullet}\ .
$$
### Two-way decompositions
Hence, for each observation we can split it up into 4 pieces:
$$
\begin{aligned}
y_{ij}&= \underbrace{\bar y_{\bullet\bullet}}_{\hat\mu}+ \underbrace{(\bar y_{i\bullet}-\bar y_{\bullet\bullet})}_{\hat\alpha_i} + \underbrace{(\bar y_{\bullet j}-\bar y_{\bullet\bullet})}_{\hat\beta_j} + \underbrace{(y_{ij}-\bar y_{i\bullet}-\bar y_{\bullet j}+\bar y_{\bullet\bullet})}_{\hat\varepsilon_{ij}}
\end{aligned}
$$
- The final part $\hat \varepsilon_{ij}$ is the $(i,j)$-th residual or estimated error

### Decomposing the Total Sum of Squares
$$
\begin{aligned}
\sum_{i=1}^g\sum_{j=1}^n(y_{ij}-\bar y_{\bullet\bullet})^2 &= \sum_{i=1}^g\sum_{j=1}^n\left\{(\bar y_{i\bullet}-\bar y_{\bullet\bullet}) + (\bar y_{\bullet j}-\bar y_{\bullet\bullet})+ (y_{ij}-\bar y_{i\bullet}-\bar y_{\bullet j}+\bar y_{\bullet\bullet}) \right\}^2 \\
&= \sum_{i=1}^g n (\bar y_{i\bullet}-\bar y_{\bullet\bullet})^2 + \sum_{j=1}^n g(\bar y_{\bullet j}-\bar y_{\bullet\bullet})^2 + {} \\
& \qquad \sum_{i=1}^g\sum_{j=1}^n\ (y_{ij}-\bar y_{i\bullet}-\bar y_{\bullet j}+\bar y_{\bullet\bullet})^2 + {}\\
& \qquad \text{cross-product terms which are all zero}\\
&= \text{Treatment sum of squares} + {}\\
& \qquad \text{Block sum of squares} + {}\\
& \qquad\text{Residual sum of squares}
\end{aligned}
$$
## Two-way ANOVA table
![[Pasted image 20230921220539.png]]
- Note that the residual degrees of freedom is different because we allocated some of those degrees of freedom to the blocks

### Purpose of Blocking
We can kind of think that a two-way ANOVA with blocking as a generalization of the paired $t$-test where each pair is a block

In a paired $t$-test, we have a before and after measurement for one individual but here we have electrode one and electrode two for one individual.
- If we want to concentrate on electrode one and electrode two, we can take the differences for each person between electrode one and electrode two
- We're accounting for the fact that we have the same person being used to measure the different electrodes on
## Behavior of Sums of Squares under the Two-Way ANOVA Model

### Two-Way ANOVA Sums of Squares Behavior
Recall that the observation at $y_{ij}$-th index (observation in block $j$ receiving treatment level $i$) is modelled as:
$$
Y_{ij} = \mu+\alpha_i+\beta_j + \varepsilon_{ij}
$$
$$
\begin{aligned}
\alpha_i & = \text{ adjustment for treatment level } i,\\
\beta_{j} & = \text{ adjustment for block }j, \\
\varepsilon_{ij} & \sim {\mathcal N}(0,\sigma^2)\,,
\end{aligned}
$$
- Note that all random variables are independent and follows the following the constraints:
$$
\sum_{i=1}^g\alpha_i = 0\ \text{ and } \ \sum_{j=1}^n\beta_j=0\,.
$$
#### Treatment Sum of Squares (Not Examinable)
- Treatment sum of squares is:
$$
\begin{aligned}
\sum_{i=1}^gn(\bar Y_{i\bullet}-\bar Y_{\bullet\bullet})^2 =  n\sum_{i=1}^g(\alpha_i+\bar \varepsilon_{i\bullet}-\bar \varepsilon_{\bullet\bullet})^2\,.
\end{aligned}
$$
- And under the null hypothesis $H_0: \alpha_1,...,\alpha_g = 0$
$$
\begin{aligned}
n\underbrace{\sum_{i=1}^g(\bar \varepsilon_{i\bullet}-\bar \varepsilon_{\bullet\bullet})^2}_{\sim \frac{\sigma^2}{n}\chi^2_{g-1}}\sim n \left( \frac{\sigma^2}{n}\chi^2_{g-1} \right)\sim \sigma^2 \chi^2_{g-1}\,
\end{aligned}
$$
- Since under the model the, $\bar{\varepsilon}_{i\bullet}$'s are iid normal with variance $\sigma^2 / n$
	- Note that this is the same as for the on-way ANOVA
#### Residual Sum of Squares (Not Examinable)
$$
\begin{aligned}
\underbrace{\sum_{i=1}^g\sum_{j=1}^n (\varepsilon_{ij}-\bar \varepsilon_{i\bullet})^2}_{\sim \sigma^2\chi^2_{N-g}} & = \underbrace{\sum_{j=1}^n g(\bar \varepsilon_{\bullet j}-\bar\varepsilon_{\bullet\bullet})^2}_{\sim \sigma^2\chi^2_{n-1}} + \underbrace{\sum_{i=1}^g\sum_{j=1}^n (\varepsilon_{ij}-\bar \varepsilon_{i\bullet}-\bar \varepsilon_{\bullet j}+\bar \varepsilon_{\bullet\bullet})^2}_{\sim ???}
\end{aligned}
$$
It looks a lot but in a nutshell, it says that our one-way residual sum of squares is equal to the Block sum of squares + the Two-way residual sum of squares
$$
\text{One-way Res SS} = \text{Block SS of Errors } + \text{ Two-way Res SS}
$$
And it can be shown that the two terms on the RHS are independent so, the last double sum **MUST BE**: $\sigma^2\chi^2_{N-g-(n-1)}\sim\sigma^2\chi^2_{(n-1)(g-1)}$

### Two-Way ANOVA F-Ratio
In summary:
- The **residual sum of squares** will ALWAYS follow a $\sigma^2\chi^2_{(n-1)(g-1)}$ distribution regardless with the $H_0$ is true or not
- The **treatment sum of squares** will **ONLY** follow a $\sigma^2\chi^2_{g-1}$ distribution if the null hypothesis is true
Hence from what we gathered above, if the null hypothesis is true, the $F$-ratio:
$$
\frac{\text{Treatment mean square}}{\text{Residual mean square}} \sim \frac{\chi^2_{g-1}/(g-1)}{\chi^2_{(n-1)(g-1)}/(n-1)(g-1)}\sim F_{g-1,\,(n-1)(g-1)}\,,
$$
- Otherwise, it tends to take larger values if the null hypothesis is NOT TRUE (??)

## Post Hoc Test and Multiple Comparisons Methods
### Bonferroni Method
Under our new parameters for the two-way ANOVA test, each "treatment difference" e.g. $\alpha_1 - \alpha_2$ is still naturally estimated using the corresponding treatment level mean differences
- Under this two-way model, the corresponding random mean difference is distributed as:
$$
\bar Y_{1\bullet}-\bar Y_{2\bullet}\sim \mathcal{N}\left( \alpha_1-\alpha_2,\ \frac{2\sigma^2}{n} \right)\,,
$$
- Since all treatment groups have a common sample size $n$
#### Individual $t$-test and confidence interval
Assuming that we are testing with a significance level, $\alpha$:
- We estimate the standard error $\sigma \sqrt{\frac{n}{n}}$ by plugging in the residual mean square as an estimate of $\sigma^2$
- suppose $c(\alpha)$ satisfies $P(-c(\alpha)\leq t_{(n-1)(g-1)}\leq c(\alpha))=1-\alpha$
- An individual level $\alpha$ $t$-test for comparing groups 1 and 2 would therefore reject for 
$$
\frac{|\bar y_{1\bullet}-\bar y_{2\bullet}|}{\hat{\sigma}\sqrt{\frac{2}{n}}}>c(\alpha)\,.
$$
- A individual 100(1-a)% confidence interval for $\alpha_1 - \alpha_2$ would be given by:
$$
\bar y_{1\bullet}-\bar y_{2\bullet}\pm c(\alpha)\,\hat{\sigma}\sqrt{\frac{2}{n}}\,.
$$
#### Adjusting for multiplicity
- If we are performing $k$ simultaneous comparisons, we place $\alpha$ with $\alpha / k$
- This means that we simply replace $c(\alpha)$ with $c(\alpha/k)$ and:
	- Perform each $t$-test at "level $\alpha/k$"
	- Construct each confidence interval at the $100(1-\alpha/k)\%$ confidence level
- After adjusting of the $k$ procedures taken simultaneous at a significance level $\alpha$:
	- The probability of incorrectly reject any of the $t$-test is no more than $\alpha$
	- The probability that all true values are included in the corresponding confidence interval is $1-\alpha$
- We will reject each test if and only if each individual unadjusted p-value is less than $\alpha / k$
	- Equivalent to rejecting if $k \times \text{p-value} > \alpha$
### Bonferroni Test
 If we are doing all pairwise comparisons across $g$ groups then there are $\binom g2$ of these pairwise comparisons and if **none** of the tests end up rejecting after adjusting for multiplicity, then the the smallest individual p-value is greater than $\alpha / \binom g2$
 - The p-value for the Bonferroni test is simply $\binom g2$ times the smallest unadjusted p-value
If this test rejects, we can do a "post hoc" to identify which comparisons are significant by identifying which adjusted p-values are less than $\alpha$

### Emmeans Package
We can use the Emmeans package to adjust the p-values for our two way ANOVA test
```
fit2 = aov(y ~ Subject + electrode, data = resist_long)
anova(fit2)
```
![[Pasted image 20230921225233.png]]
```
library(emmeans)
fit2_emmeans = emmeans(fit2, ~ electrode)
contrast(fit2_emmeans, method = "pairwise", adjust = "bonferroni")
```
![[Pasted image 20230921225246.png]]
### Tukey's Method
We can also use Tukey's method to adjust the p-values
```
# TukeyHSD(fit2, which = "electrode") # an alternative not using emmeans
contrast(fit2_emmeans, method = "pairwise", adjust = "tukey")
```
![[Pasted image 20230921225401.png]]
- The p-value for the "Tukey test" is 0.061

#### Scheffe's method
- Allows for data snooping and can be used with any number and type of contrast
```
contrast(fit2_emmeans, method = "pairwise", adjust = "scheffe")
```
![[Pasted image 20230921225536.png]]
### Summary: two-way ANOVA normal model
- Apart from the fact that we have a different $\hat{\sigma}$, everything is much the same as in the one-way ANOVA normal model
- Once we adjusted for blocks, we adjust the degrees of freedom and proceed normally
#### Checking the Assumptions
The main assumptions are **normality** and **constant variance** assumption which are usually checked by:
- Boxplot and QQ of residuals for normality
- Plotting **residuals** against **fitted values** to check common variance assumption
	- **Fitted Values**: $\hat{y}_{ij} = \hat{\mu} + \hat{\alpha}_i + \hat{\beta}_j$
	- **Residuals**: $r_{ij} = y_{ij} - \hat{y}_{ij}$

## Adjusting for Blocks using Ranks

### Kruskal-Wallis Test
Recall that in this test, we pulled all of our data, ranked it and put those ranks back into their groups and then we did an ANOVA on the ranks
- There is an two-way ANOVA equivalent called the Friedman Test
### Friedman Test
In this test, each observation is replaced by its within-block rank and does a one-way ANOVA $F$-test on the ranks
- Once we get our test statistic we can compare to a $\chi^2$-approximation or a permutation test

The equivalent statistic is:
$$
\frac{\text{Treatment sum of squares of the ranks}}{\text{Total sum of squares of the ranks/$n(g-1)$}}\,
$$
- $\chi^2$-approximation method
```
friedman.test(y ~ electrode | Subject, data = resist_long)
```
![[Pasted image 20230921230601.png]]
- Permutation/Simulation approach
```
fried.stat = friedman.test(y ~ electrode | Subject, data = resist_long)$statistic
B = 1000
fr.st = vector("numeric", length = B)
for(i in 1:B) {
  fr.st[i] = friedman.test(sample(y) ~ electrode | Subject, data = resist_long)$statistic
}
mean(fr.st>=fried.stat) # 0.244
```