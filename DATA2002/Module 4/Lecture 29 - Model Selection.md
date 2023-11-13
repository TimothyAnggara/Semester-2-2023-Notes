## Model Selection
### US Crime Rate Data
- In 1973, a study was done in 47 states of the US where the aim was to model crime rate as a function of 15 potential explanatory variables
	- We'll consider stepwise and exhaustive search schemes
#### Crime Data: Variables in the data set
![[Pasted image 20231010160920.png]]
#### Crime data: Null and Full Model
- What is the null model and full model?
```
M0 = lm(y ~ 1, data = UScrime)  # Null model
M1 = lm(y ~ ., data = UScrime)  # Full model
round(summary(M1)$coef, 3)
```
![[Pasted image 20231010161322.png]]
```
res = bind_rows(broom::glance(M1), 
                broom::glance(M0))
res$model= c("M1","M0")
res |> pivot_longer(
  cols = -model, 
  names_to = "metric", 
  values_to = "value") |> 
  pivot_wider(
    names_from = "model") |> 
  gt::gt() |> 
  gt::fmt_number(columns = 2:3, 
                 decimals = 2) |> 
  gt::fmt_missing()
```
![[Pasted image 20231010161749.png]]

## Stepwise Selection
### `drop1` and `update` command in R
- With the full model, not all variables are significant to our dataset and some can even just be clouding result and we are trying to predict.
- In order to prevent this clouding, we'll use the `drop1` function to drop one of these variables
```
drop1(M1, test = "F")
```
![[Pasted image 20231010190900.png]]
- The drop function will show us some numerical summaries about the variables we have in our model
- To remove a variable we will use `update()` function.
	- We'll remove `So` because it has the largest F-statistic p-value and we'll create M2
```
M2 = update(M1, . ~ . - So)
```
- We'll keep removing variables until all variables in the current model are important
### The Akaike Information Criterion
- A unit of measurement of how fit our model is but it is not the best or recommended method to use.
	- The smaller the AIC, the better the model
- The AIC was introduced by Hirotugu Akaike and is defined as,
$$
{\text{AIC}} = n\log\left(\frac{\text{Residual sum of squares}}{n}\right) + 2p
$$
### Crime Data: Backward Search using AIC
- In R, instead of having to manually investigate each step, we can use the `step` function
```
step.back.aic = step(M1, 
                     direction = "backward", 
                     trace = FALSE)
round(summary(step.back.aic)$coef,3)
```
![[Pasted image 20231010191231.png]]
- Using `trace = TRUE` will show the sequence of models considered in each step
- Note that if we tried to drop any variables from the `step.back.aic` model, it would increase the AIC value which is not what we want
	- We want the AIC value to be as low as possible
```
drop1(step.back.aic, test = "F")
```
![[Pasted image 20231010191339.png]]
### Forward Variable Selection
- Another way we can create our model is by starting with a model containing no explanatory variables and in each step we add a new variable which supplies the most information, (lowest AIC)
	- `lm(y ~ 1, data = dat)`
```
M0 = lm(y ~ 1, data = UScrime)  # Null model
M1 = lm(y ~ ., data = UScrime)  # Full model
step.fwd.aic = step(M0, scope = list(lower = M0, upper = M1),
                    direction = "forward", trace = FALSE)
summary(step.fwd.aic)
```
![[Pasted image 20231010191456.png]]
#### `add1()` Function
- The R function `add1(M0, scope = ~ x1 + x2 + ... + xk, dat = dat, test = "F")` will return a number of information criteria for all variables specified under the option `scope`
```
add1(step.fwd.aic, test = "F", scope = M1)
```
![[Pasted image 20231010191716.png]]

## Exhaustive Searches
- In Exhaustive searching, we will try and look at the full set all possible models
	- Note that this method is typically only feasible if $p < 100$
```
library(leaps)
exh = regsubsets(y~., data = UScrime, nvmax = 15)
summary(exh)$outmat
```
![[Pasted image 20231010191824.png]]
