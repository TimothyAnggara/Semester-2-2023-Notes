## Prediction
### Recall: Ozone Model
```
library(tidyverse)
data(environmental, package = "lattice")
environmental = environmental |> 
  mutate(lozone = log(ozone))
lm3 = lm(lozone ~ radiation + temperature + wind, environmental)
lm3
```
![[Pasted image 20231009160113.png]]
$$
\widehat{\log(\text{ozone})} = -0.26 + 0.0025\, \text{radiation} + 0.049\, \text{temperature} - 0.0616\, \text{wind}
$$
### Predictions In R
- In order to predict, we'll first need to create a new data frame with the given data we want and use the `predict` function:
```
new_obs = data.frame(radiation = 200, temperature = 90, wind = 15)

# There are two ways of prediction: prediction interval and Confidence Interval

predict(lm3, new_obs, interval = "prediction", level = 0.90)
# Fit = 3.742554, lwr = 2.867, upr = 4.618
predict(lm3, new_obs, interval = "confidence", level = 0.90)
# Fit = 3.7426, lwr = 3.5103, upr = 3.9748
```
### Two kinds of "prediction"
- Estimating the average log ozone value for specific conditions
- Estimating the log ozone for a specific day with the specific conditions
### Prediction vs Confidence Intervals
- For finding the variation for the mean estimator we use the formula:
$$
\widehat{\operatorname{Var}}(\hat y_0) = \hat{\sigma}^2 \boldsymbol{x}_0'(\boldsymbol{X'X})^{-1}\boldsymbol{x}_0,
$$
- For finding the variation for a specific day, we use the formula:
$$
\operatorname{Var}(\hat e)= \hat{\sigma}^2 \boldsymbol{x}_0'(\boldsymbol{X'X})^{-1}\boldsymbol{x}_0 + \hat{\sigma}^2.
$$
### Effect of Variance on Intervals
Our population model is:
$$
\boldsymbol{Y}=\boldsymbol{X\beta}+\boldsymbol{\varepsilon}
$$
- Where $\boldsymbol{\varepsilon}\sim N_n(0,\sigma^2)$
1. Smaller the $\sigma^2$, the better the fit and hence the smaller the variances for $\hat{\beta}$ and $\hat{y_0}$
2. Larger the spread of our $x$ variables, the more information about how $Y$ responds to each $x$ variables, hence smaller variances for $\hat{\beta}$ and $\hat{y_0}$
3. Larger the sample size $n$, smaller the variances for $\hat{\beta}$ and $\hat{y_0}$
4. The closer is $x_0$ to $\bar{x}$ (The component-wise mean of the design matrix), the smaller the variances of $\hat{y_0}$

## Categorical Predictors
### Fuel Economy
The `mpg` dataset contains a subset of fuel economy data collated by the EPA
- Contains only models which had a new release every year between 1999 and 2008
```
data("mpg", package = "ggplot2")
glimpse(mpg)
```
![[Pasted image 20231009181454.png]]
### Lets take an Initial look at your data
```
mpg |> ggplot() + 
  aes(x = cty) + 
  geom_histogram(bins = 10)
```
![[Pasted image 20231009181930.png]]
```
mpg |> ggplot() + 
  aes(y = class, x = cty) + 
  geom_boxplot()
```
![[Pasted image 20231009181942.png]]
From looking at the boxplots, we can think of doing an ANOVA where our groups are about the type of car and lets do that
```
a1 = aov(cty ~ class, data = mpg)
summary(a1)
```
![[Pasted image 20231009182116.png]]
- So there is a difference between the mean of at least two groups for fuel efficiency

What should we do if we want to take a look at a multiple linear regression on this class?
- We can use `lm` function like we did for multiple regression
```
lm1 = lm(cty ~ class, data = mpg)
summary(lm1)
```
![[Pasted image 20231009182343.png]]

From the results of the multiple regression and ANOVA, we can see some similarities. 
- Degrees of freedom
- F-statistic
- P-value

And in fact, the underlying process of using ANOVA and multiple regression is the same

