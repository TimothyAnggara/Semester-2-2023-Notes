## Logistic Regression
- We want to try and predict a discrete variable and in this lecture we'll try and predict the `survival` variable present in the Titanic data frame we looked at early this semester
### Titanic Survival
Data on passengers on the RMS Titanic which excludes crew and some individual identifier variables
- `pclass` - factor with levels 1st, 2nd, 3rd
- `survived` - factor with levels `died`, `survived`
- `sex` - factor with levels `female`, `male`
- `age` - Passenger in years
- `sibsp` - number of siblings or spouses aboard
- `parch` - number of parents or children aboard
```
library(tidyverse)
# install.packages("vcdExtra")
data("Titanicp", package = "vcdExtra")
Titanicp
```
![[Pasted image 20231016161321.png]]
### Linear Regression
- Normally, for a linear regression we would have:
$$
Y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \ldots + \beta_p x_p + \varepsilon,
$$
where $\varepsilon\sim \mathcal{N}(0,\sigma^2)$
- A conditional on the vector of predictor variable ($x$'s'), on the dependent variable $Y$ also follows a normal distribution, i.e.
$$
Y_i | \boldsymbol{x}_i \sim \mathcal{N}(\boldsymbol{x}_i'\boldsymbol{\beta}, \sigma^2).
$$
 - If the dependent variable we are trying to predict is binary, a linear regression would work so well and as a result we would use a **logistic regression** instead
### Modelling Binary Data
- Since the predictor we are looking at is binary, it would make sense to model it as a Bernoulli random variable
	- Bernoulli is special case of the binomial distribution where a signal trial is conducted
$$
Y_i | \boldsymbol{x}_i \sim \operatorname{Bernoulli}(p(\boldsymbol{x}_i,\boldsymbol{\beta}))
$$
- Where the probability of $Y_i = 1$ is given by some function of our predictors, $p(x_i,\beta)$
	- We want our function based on our predictors, $p(x_i,\beta)$ to be between the interval $[0,1]$ and it will need to depend on the linear combination of our predictors $x^`_i  \beta$ 
- A common choice for this function from our predictors is the **logistic function**
$$
p(\boldsymbol{x}_i,\boldsymbol{\beta}) = \frac{\exp(\boldsymbol{x}_i'\boldsymbol{\beta})}{1+\exp(\boldsymbol{x}_i'\boldsymbol{\beta})}.
$$
### Logistic Regression
Our logistic regression models begins with,
$$
Y_i | \boldsymbol{x}_i \sim \operatorname{Bernoulli}\left(\frac{\exp(\boldsymbol{x}_i'\boldsymbol{\beta})}{1+\exp(\boldsymbol{x}_i'\boldsymbol{\beta})}\right).
$$
- If we had a new observation vector $x_0$ and we knew the $\beta$ vector, we could calculate the probability that the corresponding $Y = 1$:
	- No idea what this means???
$$
P(Y=1 | \boldsymbol{x}_0) = \frac{\exp(\boldsymbol{x}_0'\boldsymbol{\beta})}{1+\exp(\boldsymbol{x}_0'\boldsymbol{\beta})}.
$$
- If the probability is greater than 0.5, we would make a prediction $\hat{Y} = 1$, else $\hat{Y} = 0$
- The problem here is that we don't know $\beta$ and we need to estimate
	- We can estimate $\hat{\beta}$ by **iteratively reweighted least squares**
		- I have no idea what that means???
### How to fit a Logistic Regression Model?
- With our titanic dataset, we need to first convert survival to 0/1 (numerical) variable
```
x = Titanicp %>% mutate(survived = ifelse(survived == "survived", 1, 0))
glimpse(x)
```
- We treat survived as `1` and died as `0`
- Next, to actually create we model we do very similar what we did with a normal linear regression model but we add the parameter `family = binomial`
```
glm1 = glm(survived ~ pclass + sex + age, family = binomial, data = x)
summary(glm1)
```
![[Pasted image 20231016192358.png]]

### Checking for Significance
Before we start to interpret our model, it may be a good idea if we can drop any of the variables in our model. Similarly to the linear regression, this is equivalent to testing $H_0 = \beta = 0$ against the alternative $H_1: \beta \neq 0$
- Lets do an example where we test if the coefficient for age is significantly different to zero
![[Pasted image 20231016192855.png]]
### Writing down the model
```
glm1
```
![[Pasted image 20231016192915.png]]
The fitted model is,
$$
\text{logit}(p) = 3.5 - 1.3\,\text{pclass2nd} - 2.3\,\text{pclass3rd} - 2.5\,\text{sexmale} - 0.03\,\text{Age}
$$
- Note that instead of our normal linear regression model where the LHS is our dependent variable $Y$, we have $logit(p)$ as our dependent variable
#### What is this logit function
The **logit** function is our **link** from a linear combination of the predictors to the probability of the outcome being equal to 1
$$
\operatorname{logit}(p) = \log\left(\frac{p}{1-p}\right)
$$
### Interpreting our Coefficients
$$
\log\left(\frac{p}{1-p}\right) = 3.5 - 1.3\,\text{pclass2nd} - 2.3\,\text{pclass3rd} - 2.5\,\text{sexmale} - 0.03\,\text{age}
$$
- **Intercept**: the log-odds of survival for an individual travelling in 1st class who is female and aged zero years old.
- Holding sex and age constant, the `pclass2nd` coefficient represents the **difference** in the log-odds between someone travelling in 1st class and someone travelling in 2nd class. It’s **negative**, so we’re saying that your odds of survival were lower if you travelled in second class, relative to those who travelled in first class.
- Holding class and age constant, the `sexmale` coefficient represents the **difference** in the log-odds between males and females. It is **negative,** so if you were a male, your odds of survival were **lower** than if you were a female.
- The `age` coefficient is also negative, which implies that older people had lower odds of survival than younger people. Specifically, on average, for each additional year older you are, the log-odds of survival decreased by 0.03, holding class and sex constant.

### Predictions
If we want to predict the **log-odds** for a newborn male travelling in first class:
```
new_data = data.frame(pclass = "1st", sex = "male", age = 0)
predict(glm1, newdata = new_data, type = "link") # 1.024229
```
What if we want to work out the **estimated probability** of survival for a newborn male travelling in first class?
```
new_data = data.frame(pclass = "1st", sex = "male", age = 0)
predict(glm1, newdata = new_data, type = "response")
```
## Evaluating Performance
### Making predictions
- We can make predictions by rounding our predicted probability to 0 or 1
```
x = x %>% drop_na() %>% 
  mutate(pred_prob = predict(glm1, type = "response"),
         pred_surv = round(pred_prob))
head(x, n = 10)
```
![[Pasted image 20231016193623.png]]
#### Evaluating (in-sample) performance
Now that we have our mode, how many passengers did we correctly classify?
#### Resubstitution Error Rate
- The proportion of observations we predict **incorrectly** when we try to predict all the points we used to fit the model
$$
\frac{1}{n}\sum_{i=1}^n (y_i \neq \hat{y}_i)
$$
```
mean(x$survived != x$pred_surv) # 0.2151
```
- We failed to correctly classify 21.5% of the observations

#### Confusion Matrix
We can estimate how our model predicted all the data points using `confusionMatrix` from the **caret** package
```
library(caret)
confusion.glm = confusionMatrix(
  data = as.factor(x$pred_surv), 
  reference = as.factor(x$survived))
confusion.glm
```
![[Pasted image 20231016193925.png]]
