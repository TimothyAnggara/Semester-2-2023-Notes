## Interpreting Model Coefficients
### How can we interpret the estimation coefficients?
When trying to interpret the regression, thinking about the coefficients can provide an understanding of the structure in your data ore relationships between your outcome and explanatory variables
$$
Y = \beta_0 + \beta_1 x + \varepsilon
$$
When thinking about our regression equation,
- we have $Y$, which is some outcome from some linear function of $x$
- Intercept, $\beta_0$
	- The intercept is what we'd expect $Y$ to be when $x = 0$ 
- Slope coefficient $\beta_1$
	- Slope is the amount we expect $Y$ to change as $x$ changes
- Error term, $\epsilon$
### Recall our fitted Model
```
library(tidyverse)
data(environmental, package = "lattice")
environmental = environmental |> 
  mutate(lozone = log(ozone))
lm2 = lm(lozone ~ temperature, data = environmental)
lm2
```
![[Pasted image 20231005222913.png]]
The problem with this equation that it kind of doesn't make sense. If temperature is 0, the log(ozone) would be 0 which is not good
- What we can interpret from this slope is that one degree increase in temperature results a 0.07 unit increase in log ozone, on average

### Interpreting Models with Log Transformations
- Log-linear -$\quad \log(Y) = \beta_0 + \beta_1 x$
	- On average, one unit increase in x will results a $\beta_1 \times 100\%$ change in $Y$
- Linear-log $\quad Y = \beta_0 + \beta_1 \log(x)$
	- On average, one percent increase in $x$ will result in a $\beta_1/100\%$ change in $Y$
- Log-Log -$\quad \log(Y) = \beta_0 + \beta_1 \log(x)$
	- On average, one percent increase in $x$ will result in a $\beta_1\%$ change in $Y$
## Inference In Regression Models
### Inference
The thing that we probably wanna test when we are thinking about our regression is the null hypothesis that $\beta_1$ or the slope coefficient equal to 0
- $H_0: \beta_1 = 0$
- $H_1: \beta_1 = 1$
	- If the slope is 0, then that means there is no relationship between $x$ and $Y$

So the way that we test this is using a $t$-test:
$$
T = \frac{\hat{\beta}_1 - \beta_1}{\text{SE}(\hat{\beta}_1)} \sim t_{n-2}
$$
```
summary(lm2)
```
![[Pasted image 20231005224644.png]]
### Testing for Significance of the Slope Parameter $\beta_1$
![[Pasted image 20231005224745.png]]

### P-values mean nothing if you haven't looked at your data!
Please do try and graph your data points while interpreting you data. 
![[Pasted image 20231005225039.png]]

### CI for Regression Coefficients
What to do if we want a confidence interval for our parameter estimate? 
- What's a plausible range of values for the slope parameter
$$
\hat{\beta}_1 \pm t^\star \times \text{SE}(\hat{\beta}_1)
$$
- Where $t^*$ is the $\alpha/2$ quantile from a $t$ distribution with $n-2$ degrees of freedom
```
summary(lm2)$coefficients |> round(4)
```
![[Pasted image 20231005225700.png]]
```
qt(0.025, df = 109) |> round(3) # -1.982
```

- We can plug the values we found from the code above and find the confidence Interval:
$$
0.0677 \pm 1.982 \times 0.0058 = (0.056, 0.079).
$$
## In-Sample Performance
### Decomposing the Error
- Total SS is the total variation in $Y$
- Reg SS is the sum of squares explained by the regression line
- Resid SS = Total SS - Reg SS is the variation in $Y$ remain unexplained
$$
\begin{aligned}
\underbrace{\sum_{i=1}^n(y_i-\bar y)^2}_{\textrm{Total SS}} = & \underbrace{\sum_{i=1}^n(\hat y_i-\bar y)^2}_{\textrm{Reg SS}} + \underbrace{\sum_{i=1}^n (y_i-\hat y_i)^2}_{\textrm{Resid SS}}
\end{aligned}
$$
### Coefficient of determination $r^2$
Square of correlation coefficient $r^2$ is called the coefficient of termination
- It measures the proportion of total variation in $Y$ that can be explained by the linear regression model
$$
r^2 = 1 - \frac{\sum_i (y_i -\hat y_i)^2}{\sum_i (y_i-\bar y)^2} = 1 - \frac{\textrm{Resid SS}}{\textrm{Total SS}}.
$$
The coefficient of determination, $r^2$, measures the strength of the linear relationship between $x$ and $y$ by the percentage of variation in $y$ explained by the linear regression model in $x$
### Air Pollution
```
summary(lm2)
```
![[Pasted image 20231005224644.png]]
- The $r^2$ in the ozone example is 0.5548
- We can interpret this as saying that temperature explains 55% of the observed variation in log ozone

## Multiple Regression
We can extend for regression to add extra variables and it pretty easy. Let's use the Air Pollution example
$$
\log(\text{ozone})_i = \beta_0 + \beta_1 \text{radiation}_i + \beta_2 \text{temperature}_i + \beta_3 \text{wind}_i + \varepsilon_i
$$
```
lm3 = lm(lozone ~ radiation + temperature + wind, environmental)
summary(lm3)$coefficients |> round(3)
```
![[Pasted image 20231005231310.png]]
$$
\widehat{\log(\text{ozone})} = -0.261 + 0.003\, \text{radiation} + 0.049\, \text{temperature} - 0.062\, \text{wind}
	x`$$

Multiple regression is a extension of the simple linear regression that incorporates multiple explanatory variable with the general form
$$
Y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \ldots + \beta_p x_p + \varepsilon, \text { where }\varepsilon\sim \mathcal{N}(0,\sigma^2).
$$
### Interpretation
Estimated coefficients ( $\hat{\beta}$ ) are now interpreted as "conditional on" the other variables are constant
$$
\widehat{\log(\text{ozone})} = -0.261 + 0.003\, \text{radiation} + 0.049\, \text{temperature} - 0.062\, \text{wind}
$$
- A one degree Fahrenheit increase in temperature results in a 4.9% **increase** in ozone on average, holding radiation and wind speed constant.
- A one langley increase solar radiation results in a 0.3% **increase** in ozone on average, holding radiation and wind constant.
- A 10 langley increase solar radiation results in a 3% **increase** in ozone on average, holding radiation and wind constant.
- A one mile per hour increase in average wind speed results in a 6.2% **decrease** in ozone on average, holding radiation and temperature constant.
