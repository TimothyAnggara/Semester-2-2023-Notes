## In Sample Performance
### Recall our Fitted Ozone Model
```
data(environmental, package = "lattice")
environmental = environmental |> 
  mutate(lozone = log(ozone))
lm2 = lm(lozone ~ temperature, data = environmental)
lm3 = lm(lozone ~ radiation + temperature + wind, environmental)
```
### In Sample Performance vs Out of Sample Performance
- In sample
	- We can use the $r^2$ to compare simple linear regression to the full model
	- But this does not protect against over fitting
- Out of sample?
	- How can we predict observations that we didn't use to build the model?

## Out of Sample Performance
### Comparing simple linear regression with the full model
#### Out of Sample Performance
- What we can do from our data set is to build a **training** set and use it to predict observations from a test test
```
n_train = floor(0.8*n) # Creating the training set
n_test = n - n_train # Creating the testing test

grp_labs = rep(c("Train","Test"), times = c(n_train, n_test)) 
environmental$grp = sample(grp_labs)
train_dat = environmental |> filter(grp == "Train")
lm_simple_train = lm(lozone ~ temperature, data = train_dat)
lm_full_train = lm(lozone ~ radiation + temperature + wind, data = train_dat)
test_dat = environmental |> filter(grp == "Test")
simple_pred = predict(lm_simple_train, newdata = test_dat)
full_pred = predict(lm_full_train, newdata = test_dat)
```
#### Root Mean Square Error
- We can compare prediction from our two models by using the Root mean square error:
$$
\text{RMSE} = \sqrt{\frac{\sum_{i=1}^n(y_i - \hat{y}_i)^2}{n}}
$$
```
simple_mse = mean((test_dat$lozone - simple_pred)^2)
sqrt(simple_mse) # 0.6221

full_mse = mean((test_dat$lozone - full_pred)^2)
sqrt(full_mse) # 0.499
```

### Mean Absolute Error
- An alternative measure of performance, less influenced by outliers 
$$
\text{MAE} = \frac{\sum_{i=1}^m |y_i - \hat{y}_i|}{m}
$$
```
simple_mae = mean(abs(test_dat$lozone - simple_pred))
simple_mae # 0.5149

full_mae = mean(abs(test_dat$lozone - full_pred))
full_mae # 0.3972
```

#### $k$-fold cross-validation (CV) estimation
In this method, data is randomly divided into $k$ subsets of equal size and we will estimate our model by leaving one of the subsets out and by doing so we will use our estimated model to predicted the observations left out and repeat over all the subsets and average the $k$ runs
![[Pasted image 20231013174025.png]]
### 10-Fold cross validation
1. Divide our data up into 10 folds but since the data that we are using, the ozone example, has 111 observation, we'll use 9 folds of 11 observations and 1 fold of 12
```
set.seed(2)
environmental$grp = NULL # remove the grp variable we added previously
fold_id = c(1, rep(1:10, each = 11))
environmental$fold_id = sample(fold_id, replace = FALSE)
head(environmental)
```
2. Estimate the model leaving one fold out, making predictions on the test set and calculate the error rate
```
k = 10
simple_mse = full_mse = vector(mode = "numeric", length = k)
simple_mae = full_mae = vector(mode = "numeric", length = k)
for(i in 1:k) { 
  test_set = environmental[fold_id == i,]
  training_set = environmental[fold_id != i,]
  simple_lm = lm(lozone ~ temperature, data = training_set)
  simple_pred = predict(simple_lm, test_set)
  simple_mse[i] = mean((test_set$lozone - simple_pred)^2)
  simple_mae[i] = mean(abs(test_set$lozone - simple_pred))
  full_lm = lm(lozone ~ radiation + temperature + wind, data = training_set)
  full_pred = predict(full_lm, test_set)
  full_mse[i] = mean((test_set$lozone - full_pred)^2)
  full_mae[i] = mean(abs(test_set$lozone - full_pred))
}
```
3. Aggregate the errors over the 10 folds
```
cv_res = tibble(simple_mse, full_mse, 
                simple_mae, full_mae)
cv_res
```
![[Pasted image 20231013174341.png]]

```
cv_sum_res = cv_res |> 
  summarise(
    across(.cols = everything(), 
           mean)
  )
cv_sum_res |> t()
```
![[Pasted image 20231013174351.png]]
### Caret (Classification And REgression Training)
```
library(caret)
cv_full = train(
  lozone ~ radiation + temperature + wind, environmental,
  method = "lm",
  trControl = trainControl(
    method = "cv", number = 10,
    verboseIter = FALSE
  )
)
cv_full
```
![[Pasted image 20231013174525.png]]
```
cv_simple = train(
  lozone ~ temperature, 
  environmental,
  method = "lm",
  trControl = trainControl(
    method = "cv", number = 10,
    verboseIter = FALSE
  )
)
cv_simple
```
![[Pasted image 20231013174602.png]]