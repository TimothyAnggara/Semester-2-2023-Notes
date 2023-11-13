## Nearest Neighbors
### Microchip Test Data
The `Microchips` dataset contains test scores `Test1` and `Test2` and `Label` indicating whether or not the chip passed the test
```
ggplot(data) +
  aes(x=Test1, y=Test2, 
      color=factor(Label)) + 
  geom_point(size=5, alpha = 0.8)
```
![[Pasted image 20231024131158.png]]
- From the diagram we see here, we can't really use a logistic regression or decision trees
	- We can use logistic regression to **classify** binary observations but not feasible when
		- High class separation in our data (perfect separation)
		- Non-linear combination of predictors influencing our response
	- We can use decision trees to **classify** observations into 2 or more classes, but this
		- Can be complicated
		- May overfit
		- Only draws boundaries parallel to axes
### K-nearest Neighbors Algorithm
- A non-parametric algorithm
- Votes on the class of a new data point by looking at the majority class of the `k` nearest neighbours
![[Pasted image 20231024131421.png]]
### K-Nearest Neighbors
- Suppose we have a set of training data $(X, y)$
- For some positive integer $k$, the $k$ nearest neighbor algorithm classifies a new observation $x_0$ by:
	- Finding the $k$ observations in the training data $X$$, that are closest to $x_0$ according to some distance metric
	- Denote the set of $k$ closest observations as $D(\boldsymbol{x}_{0}) \subseteq (\boldsymbol{X},\boldsymbol{y})$
	- Define an aggregation function $f$ and compute $\hat{y} = f(y)$ for the $k$ values of $y$ in $D(x_0)$
### How to measure closeness?
We can use the Euclidean distance formula to find the distance between two points
$$
d(\boldsymbol{x},\boldsymbol{z}) = \sqrt{(x_{1} - z_{1})^2 + (x_{2} - z_{2})^2 + \ldots + (x_{p} - z_{p})^2}
$$
#### Advantages:
- Easy to understand and implment
- kNN doesn't need to pre-process the training data to make prediction
- Only requires two parameters, $k$ and the distance metric
#### Disadvantages
- Distance only makes sense of quantitative variables (not categorical predictors)
- Does not scale well and predictions can be slow
- Need to store the full dataset
- Doesn't perform well with high dimensional inputs
	- Many predictors
- Easy to overfit / underfit:
	- Small $k$ can overfit
	- Too High $k$ can underfit
### Choosing $k$
- An appropriate choice of $k$ will depend on the application and the data
- Cross validation ca be used to optimize the choice of $k$
- The misclassification rate increases as $k$ increase

### kNN in R
- We first split our predictors (X) and response (Y)
```
library(class)
X = data |> 
  dplyr::select(Test1, Test2)
y = as.factor(data$Label)
```
- Then we use knn to predict a grid of new data points
```
new.X = expand.grid(
  Test1 = seq(min(X$Test1 - 0.5),
              max(X$Test1 + 0.5), 
              by = 0.1),
  Test2 = seq(min(X$Test2 - 0.5), 
              max(X$Test2 + 0.5),
              by = 0.1)
  )
```
Now, we call the `knn` function and put our original observations and their observed classes as well as the grid of "new points" we will be predicting. Here we let $k = 3$
```
pred_k3 = knn(train = X, test = new.X, 
              cl = y, k = 3, prob = TRUE)
pred_prob_k3 = attr(pred_k3, "prob")
```
![[Pasted image 20231024132342.png]]

## Performance Assessment
### In sample confusion matrix
- For $k = 3$
```
k3 = knn(train = X, test = X, 
         cl = y, k = 3)
confusionMatrix(k3, y)$table
```
![[Pasted image 20231024132626.png]]
```
confusionMatrix(k3, y)$overall[1] |> 
  round(2) # Accuracy = 0.86
```

- For $k = 9$
```
k9 = knn(train = X, test = X, 
         cl = y, k = 9)
confusionMatrix(k9, y)$table
```
![[Pasted image 20231024132711.png]]
```
confusionMatrix(k9, y)$overall[1] |>
  round(2) # Accuracy = 0.83
```
### Out of Sample k-fold cross validation to help choose $k$
- Here, to decide what k to use, we repeat the CV process to get a range of error values
- The reason we do this is because if we just do one k-fold cross validation, we can get very different results and by repeating the cross validation and finding a range of error values, we can find the bet $k$ to use
```
library(caret)
fitCtrl = trainControl(method = "repeatedcv", number = 5, repeats = 100)
set.seed(1)
knnFit1 = caret::train(factor(Label) ~ ., data = data, method = "knn", 
                       trControl = fitCtrl)
knnFit1
```
![[Pasted image 20231024132836.png]]

## Random Forest
- In a random forest, we grow many trees and each tree learns from a different (sub) samples of observations and different combinations of variables
- A random forest is constructed by
1. Choosing the number of decision trees to grow and the number of variables to consider in each tree
2. Randomly select the rows of the data frame with replacement
3. Randomly selecting the appropriate number of variables from the data frame
4. Building a decision tree on the resulting data set
5. Repeating this procedure a large number of times
6. A prediction is made by majority rule
	- That is, running your new observation through all the trees in the forest and seeing which class is predicted most often.
The `randomForest::randomForest()` function in R defaults to 500 trees which are trained on `sqrt(p)` where `p` is the number of predictors in the full data set
- We randomly select the features because if one or few features are strong predictors then those features would be selected in many of the trees causing them to be correlated thus reduce the accuracy
![[Pasted image 20231026220123.png]]
![[Pasted image 20231026220134.png]]
