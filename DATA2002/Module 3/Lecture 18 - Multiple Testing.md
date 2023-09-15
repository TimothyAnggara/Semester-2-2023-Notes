	 ## microRNA and Alzheimer's Disease
In this example, we will be examining microRNA and Alzheimer's disease
- MicroRNA are small non-coding RNA molecules that regulate gene expression
We want to ask the question if there is any evidence that microRNA behavior in the brain might be associated with Alzheimer's disease?
- Experiments measuring the amount of 309 microRNAs were done in 701 subjects
- A test of significant differences between the mean of subjects with and without Alzheimer's disease for each microRNA was done
### What does the microRNA data look like?
the microRNA data comes with two objects, `AD`and `microRNA_Data`.
- AD is an objected where the names corresponds to the subject (person) and values correspond to the presence or absence of Alzheimer's disease
```
head(AD)
# 20264936 50105725 20730959 11229148 20151388 11259716 
# 1 0 1 1 1 1
disease_status = data.frame(AD) |> 
	tibble::rownames_to_column("subject") 	
str(disease_status)

# 'data.frame': 701 obs. of 2 variables: 
# $ subject: chr "20264936" "50105725" "20730959" "11229148" ... 
# $ AD : int 1 0 1 1 1 1 0 0 1 1 ...
```
- The `microRNA_Data` object are measurements where columns are subjects and rows are the miRNAs
```
microRNA_Data[1:4,1:3]
#		20264936 50105725 20730959 
# hsa-let-7a 11.97670 12.61142 13.34787 
# hsa-let-7b 12.24761 11.77856 11.44760 
# hsa-let-7c 11.07564 11.68883 11.79726 
# hsa-let-7d 11.30709 11.50804 11.37125
```
Let's reshape the `microRNA` and merge `AD` into one tibble
```
mirna = microRNA_Data |> 
	tibble::rownames_to_column("microRNA") |>
	tidyr::pivot_longer(cols = -1, names_to = "subject", values_to = "value") |> 
	dplyr::left_join(disease_status) 

head(mirna, n = 4)
# A tibble: 4 × 4 
microRNA subject value AD 
<chr> <chr> <dbl> <int> 
1 hsa-let-7a 20264936 12.0 1 
2 hsa-let-7a 50105725 12.6 0 
3 hsa-let-7a 20730959 13.3 1 
4 hsa-let-7a 11229148 12.5 1
```
### Lots of $t$-tests
Let's use Welch's two-sample $t$-test to compare the mean for people with Alzheimer's to people without Alzheimer's for all 309 microRNA
```
mirna_res = mirna |> 
  group_by(microRNA) |> 
  summarise(pval = t.test(value~AD)$p.value)
  
mirna_res |> ggplot() + aes(x = pval) + 
  geom_histogram(boundary = 0, binwidth = 0.05, 
                 fill = "skyblue",
                 colour = "black")
```
![[Pasted image 20230911234200.png]]
```
sum(mirna_res$pval < 0.05) # 49
```
- Out of the 309 microRNA tested, 49 had a p-value less than 0.05 but are all of these "statistically significant" differences important?

### Lots of $t$-test where $H_0$ is TRUE
If there was **no association** between microRNAs and Alzheimer's disease, we would see that our p-values follow a uniform distribution (why?)
- We can generate a set of p-values knowing that there is no association and visualize this
![[Pasted image 20230913144718.png]]
- The diagram above is a example histogram where we there is no association and if we use the formula `sum(mirna$null_pval < 0.05) #15 ` 
- Even though that there are no truly important microRNAs, we can still see 15 "significant" p-values in this simulated example
### Reality of Situation
- From the example above, how do we know out of the 49 p-values which ones are real?
- We never really know what is a *real association*
	- There might be some ways to determine if the results is real or not but that depends on other external information such as
		- Replicating someone else results
		- A explanation that actually makes sense
				- In the case of the RNA, if you can make a biological argument then that's good
- A small p-value provides some evidence against the null but it could still be a false positive
	- Type 1 error

### Types of Errors
- Suppose you are testing a hypothesis that a parameter $\theta = 0$ vs the alternative where $\theta \neq 0$ 
- Type I error or false positive ($V$) conclude that $\theta$ does not equal zero when it does
- Type II error or false negative ($T$) concludes that $\theta$ equals zero when it doesn't

| Outcomes                    | Truth: $\theta = 0$ | Truth: $\theta \neq 0$ | Number of Tests |
| --------------------------- | ------------------- | ---------------------- | --------------- |
| Conclusion: $\theta = 0$    |         U            |          T              |          m - R       |
| Conclusion: $\theta \neq 0$ |            V         |                S        |  R               |
| Number of Tests  |            $m_0$         |    $m - m_0$                    |       $m$          |
 ### Error Rates:
- False positive Rate: Rate at which null results ($\theta = 0$) are called significant
$$
\operatorname{E}\left[\dfrac{V}{m_0}\right].
$$
- Family wise Error Rate (FWER): Probability of at least one false positive
$$
P(V \geq 1).
$$
- False Discovery Rate (FDR): Rate at which claims of significance are false
$$
\operatorname{E}\left[\dfrac{V}{R}\right].
$$
### Accoutning for Multiple Testing
If p-values are correctly calculated, calling all p-values less than $\alpha$ significant will control the false positive rate at level $\alpha$ on average
- Say you perform 10,000 tests and the reality is that $\theta = 0$ for all of them
- Suppose that you call all p-values less than 0.05 significant thus the expected number of false positives is: $10,000 \times 0.05 = 500$ false positives
	- Expected number of false positives = number of test below significance level $\times$ significance level
To avoid so many false positive we should control:
- Family-Wise Error Rate
- False Discovery Rate
## Controlling the family-wise error rate
### Family-Wise Error Rate
- Recall that the family wise error rate is the probability of at least one false positive
Let $T_1, ..., T_m$ be m test statistics for the null hypothesis $H_{01}$, ..., $H_{0m}$
- The FWER is the probability of falsely rejecting one or more $H_{0i}$
- $FWER = P(V\geq 1)$
If the null hypothesis is always true but we conduct $m$ tests each at significance level $\alpha$ then...
- Probability of at least one false positive is: $1 - (1-a)^m$
- e.g. if $m = 20$ then the FWER is $64\%$ 
```
m = 20
alpha = 0.05
1 - (1 - alpha)^m  
# 0.64
```
### Bonferroni Correction
- The oldest multiple testing correction
Given that a number of false positive for $m$ test is $ma$ then consider defining a new threshold for significance:
$$
a^* = \frac{a}{m}
$$
- The new threshold is "conservative" but keeps the $FWER \leq a$
- For $m = 20$, if we use the correction above our FWER will be 0.0488
```
m = 20
alpha = 0.05
1 - (1 - alpha/m)^m
# 0.0488
```
#### Basic idea of using the Bonferroni Correction 
- Suppose you do $m$ test
- You wan to control FWER at level $\alpha$ so $P( V \geq 1) \leq \alpha$
	- Probability of making one or more type I errors is controlled at level $\alpha$
- Calculate the p-values the usual way
- Set $\alpha^* = \alpha/m$
- Call all p-values less than $\alpha^*$ significant
This method of controlling the FWER is easy to do and is conservative but it may be too conservative
## Controlling the False Discovery Rate
Our aim is to keep the expected proportion of false positives in your rejected tests (FDR) close to $\alpha$
Let,
- $R$ = tota number of $H_{0i}$ rejected
- $V$ = number of $H_{0i}$ falsely rejected
- FDR = $\operatorname{E}\left(\dfrac{V}{R}\right)$
### Benjamin-Hochberg Procedure
- Most popular correction when performing lots of test in genomics, imaging, astronomy, or any other signal-processing disciplines
**How it works**:
- Suppose to do $m$ test and you want to control the FDR at level $\alpha$
- We'll calculate the p-values normally
- Order the p-values from smallest to largest
	- $p_{(1)}\leq p_{(2)} \leq\ldots \leq p_{(m)}$
- Find $j^* = \max j$ such that  $p_{(j)} \leq \dfrac{j}{m}\alpha$
- Reject all $H_{0i}$ where $p_{(i)} \leq \dfrac{j^*}{m}\alpha$
Note that this procedure for controlling the FDR is easy to calculate and less conservative which is good but it allows for more false positives and may behave strangely under dependence

Its a little complicated but basically how it works is that we rank the p-value from smallest to largest and we check at each index if it is less than $i \times \alpha / m$
```
alpha = 0.2
m = 10
p_vals = sort(mirna10$pval)
# BH procedure
# j=1: smallest p-value < 1*alpha/m?
p_vals[1] < 1*alpha/m # True

# j=2: 
# second smallest p-value < 2*alpha/m?
p_vals[2] < 2*alpha/m #True

# j=3: 
# third smallest p-value < 3*alpha/m?
p_vals[3] < 3*alpha/m # True

# j=4: 
# fourth smallest p-value < 4*alpha/m?
p_vals[4] < 4*alpha/m # False

# And so on:
```
### 10 microRNA p-values: BH method
```
result = vector(length = length(p_vals))
p_vals = sort(p_vals) # we already did this but just emphasising it
for(j in seq(p_vals)) { # seq(p_vals) is the same as 1:length(pvals)
  result[j] = p_vals[j] < j*alpha/m
}

largest_true = max(which(result == TRUE))
largest_true # 3


```
