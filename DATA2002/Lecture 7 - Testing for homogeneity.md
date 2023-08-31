## Learning Objectives:
- Testing for homogeneity in 2 $\times$ 2 tables
- Testing for homogeneity in general tables

## Testing for homogeneity in 2 $\times$ 2 tables
### COVID Treatment
A study was done where 39 plasma recipients were propensity-scored matched to 156 control patients to assess the effectiveness of convalescent plasma therapy in patients with severe or life-threatening COVID-19 at a hospital in New York City

### Test of Homogeneity
- **Purpose**: 
	To determine whether different populations have the same distribution or proportion of outcomes 
- **Context**:
	Observations are sampled from two different populations and both populations are evaluated based on the same outcomes

In the example related to COVID-19 treatment, our two populations is *treated with plasma* and *not treated with plasma* with the outcomes being *Death* and *Discharge*

Thus, we are comparing the **proportion of patients** who died **and** the proportions of patients who were **discharged** between treated **with plasma** and **without plasma**

Under the **null hypothesis**. we are always stating that there is no difference between the populations in terms of the distribution of the outcomes 
- I.e. The proportion of patients who died or was discharged is the same between treated with plasma and treated without plasma

### Two way Contingency Table

A contingency table allows us to tabulate data from multiple categorical variables and they are heavily used for research


|                     | Died | Discharged |     |
| ------------------- | ---- | ---------- | --- |
| Plasma Treatment    | 5    | 28         |  33   |
| No Plasma Treatment | 38   | 104        |   142  |
|                     | 43   | 132        |  175   |
The table above is known as a **two-way** contingency table,
- Specifically a 2 $\times$ 2 contingency table

```{r}
library(tidyverse)
dat = read_csv("covid_csv_file")
dat = dat %>% filter(outcome != "Censored") %>% 
	mutate(treatment = factor(treatment, levels = c("Plasma", "No, Plasma")), outcome = factor(outcome, levels = c("Died", "Discharged")))

table(dat$treatment, dat$outcome)
```

### Notation
In 2 $\times$ 2 contingency tables, for column $c$ and row $r$ let, 

|                     | Died     | Discharged |     |
| ------------------- | -------- | ---------- | --- |
| Plasma Treatment    | $y_{11}$ | $y_{21}$   | $y_{1\bullet} = n_1$  |
| No Plasma Treatment | $y_{12}$ | $y_{22}$   | $y_{2\bullet} = n_2$ |
|                     | $y_{\bullet1}$      | $y_{\bullet2}$        | $n = n_1 + n_2$ |
Under the null hypothesis of homogeneity, we have $p_{11} = p_{21}$ and $p_{12} = p_{22}$ so our best estimate of the proportion in each category is the column total divided by the overall sample size, $\hat{p}_{ij} = \frac{y_{\bullet j}}{n}$
- Under $H_0$, the expected counts are $e_{ij} = n_i\hat{p}_{ij} = y_{i\bullet} \frac{y_{\bullet j}}{n}$

### Chi-Square Test of Homogeneity
Using our observed counts and expected counts for each cell, we can construct a chi-square test for homogeneity:
$$
T = \sum_{i=1}^2\sum_{j=1}^2\frac{(Y_{ij}-e_{ij})^2}{e_{ij}}  \sim \chi^2_1 \quad  \textrm{approximately}.
$$
The expected cell counts are:
$$
e_{ij} = n_i \hat{p}_{ij} = y_{i \bullet}\dfrac{y_{\bullet j}}{n} = \frac{\text{Row }i\text{ total}\times \text{Column }j\text{ total}}{\text{Overall total}}
$$
Note that the expected cell counts must also be >= 5 to do the chi-squared test

![[Pasted image 20230814214805.png]]

### COVID Treatment
From the COVID study referenced above, we want to assess if there is any evidence that convalescent plasma is an effective treatment for severe COVID-19 
$$
\begin{array}{l|cc|c}
& \text{Died} & \text{Discharged} & \\ \hline
\text{Plasma treatment} & 5 & 28 & 33 \\
\text{No plasma treatent} & 38 & 104 & 142 \\ \hline
&  43 & 132 & 175 \\
\end{array}
$$
$$
{\scriptsize
\begin{array}{l|cc|c}
\text{Expected values }e_{ij} & \text{Died} & \text{Discharged} & \text{Row total} \\ \hline
\text{Plasma treatment} & \dfrac{33 \times 43}{175} = 8.11 & \dfrac{33 \times 132}{175} = 24.89 & 33 \\
\text{No plasma treatent} & \dfrac{142 \times 43}{175} = 34.89 & \dfrac{142 \times 132}{175} = 107.11 & 142 \\ \hline
\text{Column total} &  43 & 132 & 175 \\
\end{array}
}
$$
$$
{\scriptsize
\begin{align*}
t_0 & = \displaystyle\sum_{i=1}^2\sum_{j=1}^2\dfrac{(y_{ij}-e_{ij})^2}{e_{ij}} \\
& = \frac{(5 - 8.11)^2}{8.11} + \frac{(28 - 24.89)^2}{24.89} + \frac{(38 - 34.89)^2}{34.89} + \frac{(104 - 107.11)^2}{107.11}\\
& = 1.9471
\end{align*}
}
$$
![[Pasted image 20230814215144.png]]
```
library(tidyverse)
dat = read_csv("covid_csv_file")
dat = dat %>% filter(outcome != "Censored") %>% 
	mutate(treatment = factor(treatment, levels = c("Plasma", "No, Plasma")), outcome = factor(outcome, levels = c("Died", "Discharged")))

tab = table(dat$treatment, dat$outcome)
chisq.test(tab, correct=FALSE)
## X-squared = 1.9471, df = 1, p-value = 0.1629
```

## Testing for Homogeneity in General Tables

### Voter Sentiment
A survey conducted in Labor and Liberal parties were done to compare the fraction of voters favoring the new tax reform package. A random sample of 100 voters were polled in each of the Labor and Liberal parties

$$
\begin{array}{l|ccc|c}
& \text{Approve} & \text{Not approve} & \text{No comment} & \text{Row total} \\ \hline
\text{Labor} & 62 & 29 & 9 & 100 \\
\text{Liberal} & 47 & 46 & 7 & 100 \\ \hline
\text{Column total} &  109 & 75 & 16 & 200 \\
\end{array}
$$
Now we should ask if the data that is being presented is sufficient evidence that the traction of voters favoring the new tax reform package differ in Labor and Liberal parties

### A General Two-Way Contingency Table
$$
\begin{array}{c|cccc|c}
& \text{Category 1} & \text{Category 2} & \ldots & \text{Category }c & \text{Row total (fixed)} \\ \hline
\text{Population 1} & y_{11} & y_{12} & \ldots & y_{1c} & y_{1\bullet} \\
\text{Population 2} & y_{21} & y_{22} & \ldots & y_{2c} & y_{2\bullet} \\
\vdots  & \vdots & \vdots & & \vdots & \vdots \\
\text{Population }r & y_{r1} & y_{r2} & \ldots & y_{rc} & y_{r\bullet} \\ \hline
\text{Column total} & y_{\bullet 1} & y_{\bullet 2} & \ldots & y_{\bullet c} & y_{\bullet\bullet} = n
\end{array}
$$
- A contingency table allows us to be able to tabulate data from multiple categorical variables
- The contingency table above is a two-way contingency table, with $r \times c$ dimensions
- There are $rc$ categories and either row or column totals are fixed
	- hence n is also fixed

### Test of homogeneity in general Two-Way Tables

$$
\begin{array}{c|cccc|c}
& \text{Category 1} & \text{Category 2} & \ldots & \text{Category }c & \text{Row total} \\ \hline
\text{Population 1} & \color{red}{p_{11}} & \color{blue}{p_{12}} & \ldots & \color{orange}{p_{1c}} & p_{1\bullet} = 1\\
\text{Population 2} & \color{red}{p_{21}} & \color{blue}{p_{22}} & \ldots & \color{orange}{p_{2c}} & p_{2\bullet} = 1 \\
\vdots  & \vdots & \vdots & & \vdots & \vdots \\
\text{Population }r & \color{red}{p_{r1}} & \color{blue}{p_{r2}} & \ldots & \color{orange}{p_{rc}} & p_{r\bullet} = 1 \\ \hline
\end{array}
$$
Under the null hypothesis of **homogeneity** $\color{red}{p_{11}} = \color{red}{p_{21}} = \ldots = \color{red}{p_{r1}}$, $\color{blue}{p_{12}} = \color{blue}{p_{22}} = \ldots = \color{blue}{p_{r2}}$,  $\color{orange}{p_{1c}} = \color{orange}{p_{2c}} = \ldots = \color{orange}{p_{rc}}$

$$
{\scriptsize
\begin{array}{l|ccc|c}
& \text{Approve} & \text{Not approve} & \text{No comment} & \text{Population total} \\ \hline
\text{Labor} & \color{red}{y_{11}} & \color{blue}{y_{12}} & \color{orange}{y_{13}} & y_{1\bullet} = n_1 \\
\text{Liberal} & \color{red}{y_{21}} & \color{blue}{y_{22}} & \color{orange}{y_{23}} & y_{2\bullet} = n_2 \\ \hline
\text{Category total} &  \color{red}{y_{\bullet1}} & \color{blue}{y_{\bullet2}} & \color{orange}{y_{\bullet3}} & n \\
\end{array}
}
$$
So, the null hypothesis in the example with context to political parties is the proportion of Approve, Not Approve, and No comment outcomes are homogeneous across the liberal and labor parties,

$$
p_{11} = p_{21}, p_{12} = p_{22}, p_{13} = p_{23}
$$
And since we don't know the proportion of each cell, $p_{ij}$, we need to estimate using category proportions, $\hat{p}_{ij} = \frac{y_{\bullet j}}{n}$

Under $H_0$, the expected counts are:
$$
e_{ij} = n_i \hat{p}_{ij} = y_{i \bullet}\dfrac{y_{\bullet j}}{n} = \frac{\text{Row }i\text{ total}\times \text{Column }j\text{ total}}{\text{Overall total}},
$$
With the test statistic being:
$$
T = \displaystyle\sum_{i=1}^r\sum_{j=1}^c\frac{(Y_{ij}-e_{ij})^2}{e_{ij}} \sim \chi^2_{(r-1)(c-1)}
$$
### Degrees of Freedom
$$
\begin{array}{l|ccc|c}
& \text{Approve} & \text{Not approve} & \text{No comment} & \text{Row total} \\ \hline
\text{Labor} & y_{11} & y_{12} & y_{13} & y_{1\bullet } = n_1 \\
\text{Liberal} & y_{21} & y_{22} & y_{23} & y_{2\bullet } = n_2 \\ \hline
\text{Column total} &  y_{\bullet 1} & y_{\bullet 2} & y_{\bullet 3} & n \\
\end{array}
$$
For a more general contingency table, the degrees of freedom are not as straightforward as a 2x2 contingency table. For the example above, we needed to estimate 3 parameters, $\hat{p}_{\bullet 1}$,  $\hat{p}_{\bullet 2}$ and $\hat{p}_{\bullet 3}$

The degrees of freedom for a $2 \times 3$ table is 2 with the general formula being for a table with $r \times c$ dimensions:
$$
df = (r-1) \times (c-1)
$$
![[Pasted image 20230814221433.png]]
