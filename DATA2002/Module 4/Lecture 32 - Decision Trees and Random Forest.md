## Decision Trees
### Decision Trees
- A decision tree determines the predicted outcome based on a series of questions and conditions
![[Pasted image 20231018164443.png]]
### Iris Dataset Example
- WE can use the **rpart** package to build our decision tree
- It uses a formula structure similar to `lm`, `glm()` to identify the dependent variable and explanatory variables
```
library(rpart)
tree = rpart(Species ~ ., data = iris, method = "class")
```
- The `method = "class"` means we want to build a classification (decision) tree
#### Visualizing The Tree
- The `tree` variable just spits out some numbers and values but if we want to visualize those values, numbers, and probabilities, we can use the `rpart.plot()` from `rpart.plot` library
```
library(rpart)
tree = rpart(
  Species ~ .,
  data = iris,
  method = "class") 

library(rpart.plot)
rpart.plot(tree, extra = 104)
```
![[Pasted image 20231018164859.png]]
- Each node shows a predicted class and the predicted probability of each class and percentage of observations in each node
#### Alternative Visualization with Party kit
```
# install.packages("partykit") # A Toolkit for Recursive Partytioning
library(partykit)
plot(as.party(tree))
```
![[Pasted image 20231018165001.png]]

### Making Predictions
```
tree <- rpart(Species ~ ., data = iris, method = "class") 
new_data = data.frame(Sepal.Length = c(5.0, 5.0), 
                      Sepal.Width = c(3.9, 3.9),
                      Petal.Length = c(1.4, 3.4), 
                      Petal.Width = c(0.3,0.3),
                      Species = c(NA,NA))
predict(tree, new_data, type = "class")
```
- Similar to how we would predict using `lm()` we would create a data frame with our predictors and plug it into the `predict()` but we specify the `type = "class"`

### Assessing In Sample Performance
```
library(caret)
predicted_species = predict(tree, type = "class")
confusionMatrix(data = predicted_species, reference = iris$Species)
```
![[Pasted image 20231018165926.png]]
### Model Selection
- How did you tree know to know to only use two splits?
	- Two branches
- There is a **complexity parameter** that can be used to determine if a proposed new split sufficiently improves the predictive power or not
- A choice is whether to keep or "prune" a proposed new branch
- By default, the new branch should decrease the error by 1%
- This helps avoid overfitting

### Titanic Tree
```
data("Titanicp", package = "vcdExtra")
titanic_tree = rpart(survived ~ sex + age + pclass, data = Titanicp, method = "class")
plot(as.party(titanic_tree))
```
- This is the default tree
![[Pasted image 20231018170156.png]]
- This happens if we decrease the complexity parameter threshold to 0.9
```
titanic_tree0.9 = rpart(survived ~ sex + age + pclass, data = Titanicp, method = "class", 
                      control = rpart.control(cp = 0.009))
plot(as.party(titanic_tree0.9))
```
![[Pasted image 20231018170245.png]]

### Performance Benchmarking
When considering performance, we should take into account taht a "null" model might appear to give quite good performance when we have unbalanced group sizes
- If our prediction model was just that everyone died, the accuracy would be: `809/(809+500) = 0.62`
### Evaluating Out-of-Sample performance
```
titanic_complete = Titanicp |> select(survived, sex, age, pclass) |> drop_na()
train(survived ~ sex + age + pclass, data = titanic_complete,
      method = "rpart", trControl = trainControl(method = "cv", number = 10))
```
![[Pasted image 20231018170514.png]]
### Final Model
- The CV procedure suggested 1.6% for the complexity parameter
```
titanic_final = rpart(survived ~ sex + age + pclass, data = titanic_complete, 
                      control = rpart.control(cp = 0.016))
plot(as.party(titanic_final))
```
![[Pasted image 20231018170545.png]]
### Decision Tree Weakness
- The problem with decision trees is that they can be complex very quickly
	- Without a complexity penalty, it'll happily continue until perfect classification
	- Which is massively overfitting the data
- Selected tree may be sensitive to the complexity penalty
- Decision trees can only make decisions parallel to axes:
![[Pasted image 20231018170744.png]]

## Random Forest