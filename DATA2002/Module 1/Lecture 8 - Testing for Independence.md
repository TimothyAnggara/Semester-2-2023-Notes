
## Learning Objectives
- Testing for independence in 2 $\times$ 2 tables
- Testing for independence in general tables

## Testing for independence in 2 $\times$ 2 tables
### Titanic
Preloaded in R, we have the Titanic dataset which provides information on the fate of passengers on the Titanic ship. The data is summarized according to economic status, sex, age, and survival
![[Pasted image 20230815160313.png]]
### Test for Independence
There are times where we sample from a population and the information gathered can be categorized according among two categorical variables and each categorical variable has various levels.

The question we are asking is whether the categorical variables are independent and we can summarize what we know using a contingency table:

$$
\begin{array}{l|cc|c}
& \text{Survive} & \text{Did not survive} & \text{Row total} \\ \hline
\text{Male} & y_{11} & y_{12} & y_{1\bullet}\\
\text{Female} & y_{21} & y_{22} & y_{2\bullet}\\ \hline
\text{Column total} &  y_{\bullet 1} & y_{\bullet 2} & n\\
\end{array}
$$
### Table of Proportions
Let $p_{ij}$ denote the probability of an observation falling into the cell with index $(i,j)$.
$$
p_{i\bullet}=\sum_{j=1}^2 p_{ij}\quad \text{and}\quad p_{\bullet j}=\sum_{i=1}^2p_{ij}
$$
$$
\begin{array}{l|cc|c}
& \text{Survive} & \text{Did not survive} & \text{Row total} \\ \hline
\text{Male} & p_{11} & p_{12} & p_{1\bullet}\\
\text{Female} & p_{21} & p_{22} & p_{2\bullet}\\ \hline
\text{Column total} &  p_{\bullet 1} & p_{\bullet 2} & 1\\
\end{array}
$$
### Independence
Two events or variables are said to be independent if:
$$
P(X=x~|~Y=y) = P(X=x) \ \text{ or } \ P(X=x, Y=y) = P(X=x)P(Y=y)
$$
And in the context of our $2 \times 2$ contingency table, we are asking whether the the survival and gender variable are independent to each other or not by asking:
$$
\begin{array}{l|cc|c}
& \text{Survive} & \text{Did not survive} & \text{Row total} \\ \hline
\text{Male} & p_{11} & p_{12} & p_{1\bullet}\\
\text{Female} & p_{21} & p_{22} & p_{2\bullet}\\ \hline
\text{Column total} &  p_{\bullet 1} & p_{\bullet 2} & 1\\
\end{array}
$$
$$
p_{11} = P(X = \text{Survive}, Y = \text{Male}) = P(X = \text{Survive})P(Y = \text{Male}) = p_{\bullet 1}p_{1\bullet}
$$
### Test Statistic
Under the null hypothesis of independence, the expected frequencies are: $e_{ij} = n p_{ij} = n p_{i\bullet} p_{\bullet j}$
- We multiply $n$ by $p_{i\bullet} p_{\bullet j}$ because we are expected that each cell are independent and by the definition of independence that the probability of that cell is equal to $P(X = x)P(Y = y)$, hence $p_{i\bullet} p_{\bullet j}$

So our test statistic for this null hypothesis given that we have a $2 \times 2$ contingency table is:
$$
T=\sum_{i=1}^2\sum_{j=1}^2\frac{(Y_{ij}- e_{ij})^2}{e_{ij}} = \sum_{i=1}^2\sum_{j=1}^2\frac{(Y_{ij}- n p_{i\bullet} p_{\bullet j})^2}{n p_{i\bullet} p_{\bullet j}},
$$
However, we do not know the probability of $p_{i\bullet}$ and $p_{\bullet j}$, hence we must estimate them by:
$\hat{p}_{i\bullet} = \frac{y_{i\bullet}}{n}$ and $\hat{p_{\bullet j}} = \frac{y_{\bullet j}}{n}$
Thus, our test statistics comes to:
$$
t_0 = \sum_{i=1}^2\sum_{j=1}^2\frac{(y_{ij}-n\,\hat p_{i\bullet}\, \hat p_{\bullet j})^2}{n\, \hat p_{i\bullet}\, \hat p_{\bullet j}} = \sum_{i=1}^2 \sum_{j=1}^2 \frac{(y_{ij}-y_{i\bullet}y_{\bullet j}/n)^2}{y_{i\bullet}y_{\bullet j}/n}.
$$
![[Pasted image 20230815174021.png]]
### Titanic Test of Independence

```
titanic_df = as.data.frame(Titanic)
head(titanic_df)
y_mat = xtabs(Freq ~ Sex + Survived,
              data = titanic_df)
chisq.test(y_mat, correct = FALSE)
## X-squared = 59.159, df = 1, p-value = < 0.0001
```
![[Pasted image 20230815180434.png]]
## Testing for Independence in General Tables
### Advertisement
200 Randomly sampled people are classified according to their income level and their reactions to an advertisement for a product:
$$
\begin{array}{l|ccc|r}
& \text{Positive}  &\text{Negative}  &\text{No opinion}& \text{Total}\\
\hline
\text{High income}   & 24 & 46 & 38 & 108 \\
\text{Low income} & 32 & 22 & 38 & 92  \\
\hline
\text{Total}  & 56 & 68 & 76 & 200
\end{array}
$$
Please note that this is different from testing for homogeneity as only the **total, bottom right,** number is fixed and **not the total row**

### General 2-way Contingency Tables
Let's now think about $r \times c$ tables with classifying factors $R$ at $r$ levels and $S$ at $c$ levels and we can have $P(R = i, S = j) = p_{ij}$ 
$$
\begin{array}{c|cccc|c}
& S=1  & S=2 & \cdots & S=c  & \text{Total} \\
\hline
R=1    & y_{11} & y_{12} & \cdots & y_{1c} & y_{1\bullet} \\
R=2    & y_{21} & y_{22} & \cdots & y_{2c} & y_{2\bullet} \\
\vdots & \vdots & \vdots &        & \vdots  \\
R=r    & y_{r1} & y_{12} & \cdots & y_{rc} & y_{r\bullet} \\
    \hline
\text{Total}    & y_{\bullet 1} & y_{\bullet 2} & \cdots & y_{\bullet c} & y_{\bullet\bullet}
\end{array}
$$

### Estimating $p_{ij}$ under independence
Suppose we have a completely random sample of size n classified by $R$ and $S$ and we want to test $H_0: R$ and $S$ are independent.

Mathematically to test if $R$ and $S$ are independent, then:
$$
p_{ij} = P(R = i, S = j) = P(R=i)(P(S=j)
$$
And to estimate $\hat{p}_{i\bullet}$ and $\hat{p}_{\bullet j}$ we will use the following formula:
$$
\widehat{p}_{i \bullet}= \displaystyle \frac{1}{n}\sum_{j=1}^c y_{ij} = \frac{y_{i\bullet}}{n}
$$
$$
\widehat{p}_{\bullet j} = \displaystyle \frac{1}{n}\sum_{i=1}^r y_{ij} = \frac{y_{\bullet j}}{n}
$$

And under the independence hypothesis, the expected frequency in the $(i,j)$ cell is:
$$
e_{ij} = n \widehat{p}_{ij} = n \widehat{p}_{i \bullet} \widehat{p}_{\bullet j} = \frac{y_{i \bullet} y_{\bullet j}}{n}
$$
### Advertisement
Back to our advertisement example, we can modify our table into the probability contingency table to get a better look on how to fill the probabilities in our table
$$
\begin{array}{l|ccc|r}
& \text{Positive} & \text{Negative} & \text{No opinion} & \text{Total}\\
\hline
\text{High income}   & p_{11} & p_{12} & p_{13} & p_{1\bullet} \\
\text{Low income} & p_{21} & p_{22} & p_{23} & p_{2\bullet} \\
\hline
\text{Total} & p_{\bullet 1} & p_{\bullet 2} & p_{\bullet 3} & 1
\end{array}
$$
Remember that mathematically, two variables are event are independent if:
$$
P(X=x~|~Y=y) = P(X=x) \ \ \text{or} \ \ P(X=x, Y=y) = P(X=x)P(Y=y)
$$
Let $X$ be a random variable representing opinion and let $Y$ be a random variable representing income. Under independence:
$$
\begin{align*}
p_{12} & = P(X = \text{Negative}, Y = \text{High income}) \\
& = P(X = \text{Negative})P(Y = \text{High income}) \\
& = p_{\bullet 2}p_{1\bullet}
\end{align*}
$$
### Test Statistic
Under $H_0$ of independence, the expected frequencies are $e_{ij} = n p_{ij} = n p_{i\bullet} p_{\bullet j}$ and to find the test statistic is:
$$
T=\sum_{i=1}^r\sum_{j=1}^c\frac{(Y_{ij}-e_{ij})^2}{e_{ij}} = \sum_{i=1}^r\sum_{j=1}^c\frac{(Y_{ij}-n\,p_{i\bullet}\, p_{\bullet j})^2}{n\,p_{i\bullet}\, p_{\bullet j}}
$$
But be careful that because we have unknown parameters $p_{i\bullet}$ and $p_{\bullet j}$ we need to estimate them using:
$$
\hat p_{i\bullet}= y_{i\bullet}/n,\qquad \hat p_{\bullet j}= y_{\bullet j}/n.
$$
and so to find our test statistic for the test of independence is:
$$
t_0=\sum_{i=1}^r\sum_{j=1}^c\frac{(y_{ij}-n\,\hat p_{i\bullet}\, \hat p_{\bullet j})^2}{n\, \hat p_{i\bullet}\, \hat p_{\bullet j}} = \sum_{i=1}^r \sum_{j=1}^c \frac{(y_{ij}-y_{i\bullet}y_{\bullet j}/n)^2}{y_{i\bullet}y_{\bullet j}/n}.
$$
### Degrees of Freedom
Similar to the test of homogeneity, our degrees of freedom for a $r \times c$ table is $(r-1) (c-1)$ and thus for the example regarding the advertisement, we have a degree of freedom of 2,
- $(2-1)(3-1) = 1 \times 2 = 2$

![[Pasted image 20230815180347.png]]
### Advertisement Test of Independence
```
y = c(24,32,46,22,38,38)
n = sum(y)
c = 3
r = 2
y_mat = matrix(y, nrow = r, ncol = c)
colnames(y_mat) = c("Positive", 
                    "Negative", 
                    "No opinion")
rownames(y_mat) = c("High income", 
                    "Low income")
chisq.test(y_mat, correct = FALSE)
## X-squared = 8.3871, df = 2, p-value = 0.01509
```
![[Pasted image 20230815180405.png]]

