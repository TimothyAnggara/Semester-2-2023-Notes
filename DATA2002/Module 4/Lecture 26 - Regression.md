## Module 4: Learning and Prediction

### Types of Learning
- **Supervised Learning**
	- Knowledge of class labels or values
	- Goal: Train a model using known class labels to predict class or value label for a new data point
- **Unsupervised Learning**
	- No knowledge of output class or vauue
		- Data is unlabeled
	- Goal: Determine data patterns / groupings
![[Pasted image 20231003220615.png]]
### Supervised Learning
Supervised learning can be broken down into two main classes: **classifications** and **regression**
- Under **classification**, It maps input to an output label
	- Identifies if an object is x or y
- Under **regression**, it maps inputs to a continuous output
	- Tries to predict a continuous y value
![[Pasted image 20231003221014.png]]
## Regression
### Air Pollution
The environmental data frame has 4 environmental variables: `ozone`, `radiation`, `temperature`, `wind` taken from New York City from May to September of 1973
```
library(tidyverse)
data("environmental", package = "lattice")
# ?environmental
glimpse(environmental)
```
![[Pasted image 20231003221120.png]]
- We'd like to assess whether maximum daily temperature has an influence on average ozone concentration
```
p = ggplot(environmental) + aes(x = temperature, y = ozone) + 
  geom_point(size = 3, alpha = 0.6) + 
  labs(x = "Temperature (°F)", y = "Ozone concentration\n(parts per billion)") + geom_smooth(method = "lm", se = FALSE)
p
```
![[Pasted image 20231003222322.png]]

### Simple Linear Regression
A **simple linear regression** model aims to predict an outcome variable, $Y$, using a  single predictor variable $x$,
$$
Y_i = \beta_0 + \beta_1 x_i + \varepsilon_i
$$
Where:
- $\beta_0$ is the population intercept parameter (Y-intercept)
- $\beta_1$ is the population slope parameter
- $\epsilon_i$ is the error term typically assumed to follow $\mathcal{N}(0,\sigma^2$)
Hence,
$$
Y_i \sim \mathcal{N}( \beta_0 + \beta_1 x_i,\ \sigma^2).
$$
### Fitting a straight line by least squares
So, how can find that line of best fit and to do that we want to minimize the residuals or some function of the residuals
- What's a residual?
	- Let, $r_i = y_i - \hat{y}_i$
		- Where, $\hat{y}_i$ is the fitted value, the value we predict for the $i$-th observation given the $i$-th predictor value:
		- $\hat{y}_i = \hat{\beta}_0 + \hat{\beta}_1 x_i$
We can find the estimated intercept and estimated slope by solving the following optimization problem:
- Minimization problem to minimize  the residual sum of squares
$$
\operatorname{argmin}_{\beta_0, \beta_1}\sum_{i=1}^n (y_i - (\beta_0 + \beta_1x_i))^2.
$$
- R solves this for us with the `lm()` function
### Air Pollution
```
lm1 = lm(ozone ~ temperature, 
         data = environmental)
lm1
```
![[Pasted image 20231003223410.png]]
Our estimated model is:
$$
\widehat{\text{ozone}} = -147.646 + 2.439\times \text{temperature}
$$
### Fitted Values and Residuals
- The fitted values are obtained by plugging the observed predictor values into our estimated model, $\hat{y}_i =\hat{\beta}_0 + \hat{\beta}_1 x_i$
```
environmental = environmental |> 
  mutate(
    fitted = -147.646 + 2.439 * temperature
  )
```
- The residuals are difference between the observed outcome variable( $y$ ) and the value the estimated model predicts for that observation, the fitted value ( $\hat{y}$ )
$$
r_i = y_i - \hat{y}_i\ .
$$
```
environmental = environmental |> 
  mutate(
    resid = ozone - fitted
    )
```
## Checking Assumptions
### Linear Regression Assumptions
There are 4 assumptions underling our linear regression model:
- Linearity - Relationship between $Y$ and $x$ is linear
- Independence - All errors are independent of each other
- Homoscedasticity - Errors have constant variance 
	- $\operatorname{Var}(\varepsilon_i) = \sigma^2$ for all $i = 1,2,...,n$
- Normality - Error follows a normal distribution

### Assumption 1: Linearity
- Violations to the linearity assumptions is very serious and can mean that your predictions are likely to be **systematically wrong**
#### Checking for Linearity
1. Before the regression, plot $y$ against $x$ and look to see if the relationship is approximately linear
2. After running the regression, look at a plot of the residuals against $x$
	- Residuals should be symmetrically distribution above and below zero
	- Curved pattern in the residuals is evidence for non-linearity
		- Some values of $x$ in the model regularly overestimates $y$ while in other regions the model regularly underestimates $y$
```
p1 = environmental |> ggplot() + 
  aes(x = temperature, y = ozone) + 
  geom_point(size = 3) + 
  labs(x = "Temperature (°F)",
       y = "Ozone concentration") +
  geom_smooth(method="lm", se=FALSE)
p1

p2 = environmental |> ggplot() + 
  aes(x = temperature, y = resid) + 
  geom_point(size = 3) + 
  labs(x = "Temperature (°F)",
       y = "Residual") +
  geom_hline(yintercept = 0)
p2
```
![[Pasted image 20231003224725.png]]
- This means that we **underestimate** the ozone level for low and high temperatures and **overestimate** the ozone level at moderate temperatures
- Our predictions are **systematically wrong** for certain ranges of temperature
### Transformation 
If we see a non-linear relationship between $x$ and $y$, we might be able to transform the data so that we have a linear relationship
- What if we considered the log of ozone concentration
```
p1 + scale_y_log10()
```
![[Pasted image 20231003224535.png]]
### Air Pollution
```
environmental = environmental |> 
  mutate(lozone = log(ozone))
lm2 = lm(lozone ~ temperature, data = environmental)
lm2
```
![[Pasted image 20231003224637.png]]
Hence now our new fitted model is:
$$
\widehat{\log(\text{ozone})} = -1.84852 + 0.06767\times\text{temperature}
$$
```
p1 = environmental |> ggplot() + 
  aes(x = temperature, y = lozone) + 
  geom_point(size = 3) + 
  theme_classic(base_size = 30) + 
  labs(x="Temperature (°F)",
       y="Log ozone concentration") +
  geom_smooth(method="lm", se=FALSE)
p1

p2 = environmental |> ggplot() + 
  aes(x = temperature, y = lresid) + 
  geom_point(size = 3) + 
  theme_classic(base_size = 30) + 
  labs(x = "Temperature (°F)",
       y = "Residual") +
  geom_hline(yintercept = 0)
p2
```
![[Pasted image 20231003224831.png]]
### Assumption 2: Independence
This assumption is generally dealt with the experimental design phase - **before data collection**
- The aim is to design the experiment so that observations are not related to one another
- If you don't have a random sample, estimates of $\hat{\beta_0}$ and $\hat{\beta_1}$ may be biased
### Assumption 3: Homoscedasticity
- Homoscedasticity (homo: same, scedasticity: spread)
- Constant error variance is important to ensure the hypothesis tests can give valid results
- Violations makes it difficult to estimate the "true" standard deviation resulting in confidence intervals that are too wide or too narrow
- You can check for homoscedasticity in plots of residuals versus $x$. If it appears the residuals are getting more spread-out, that is evidence of heteroskedasticity
![[Pasted image 20231003225901.png]]

### Assumption 4: Normality
- Violations of normality of errors can compromise our inferences
	- Calculations of confidence intervals may be too wide or narrow 
- Best way to check for normality is a QQ plot of the residuals
	- 