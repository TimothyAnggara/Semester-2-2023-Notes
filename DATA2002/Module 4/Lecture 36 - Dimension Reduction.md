## Principal Component Analysis
### Sub-spaces
- Space expands exponentially with dimensions,
- Data that we receive will usually be in high dimensional space meaning having many variables 
- What we want to figure out is to reduce that dimensionality into subspaces containing the data which lowers the dimensionality
![[Pasted image 20231026223052.png]]
### PCA
- Principal component analysis produces a low-dimensional representation of a dataset.
- Finds a sequence of linear combinations of the variables that have maximal variance and are mutually uncorrelated
	- Captures as much variation as possible with as little as variables as possible
	- It's an unsupervised learning method
#### Why would we run PCA?
- In some scenarios, we may have too many predictors for a regression and as a result we can first run PCA on our dataset and then run a regression on the reduced dataset
	- This is called PCA regression
- We can also use PCA to understand relationships between variables, similar to a correlation matrix
- Data Visualization: We can plot a small number of variables more easily than a large number
### PCA: Definition
The first principal component of a set of variables $x_1, x_2, ...,x_p$ is the linear combination
$$
z_1 = \phi_{11}x_1 + \phi_{21}x_2 + \dots + \phi_{p1}x_p
$$
- Where the $\phi$ are coefficients which are trying to extract as much variance as possible from the original dataset
	- It tries to pull out the variable which the most variance from the data set
![[Pasted image 20231026224242.png]]
- In the example above, the first principal component is the full line which is trying to find the direction of maximum variance through the data
- The second principal component has to be orthogonal to the first component which also captures the maximum variance through the data

### PCA: Geometry
- The loading vector $\boldsymbol{\phi}_1 = [\phi_{11},\dots,\phi_{p1}]'$ defines the direction in space where the data vary the most
- If we project our original observations, $\boldsymbol{x}_1,\dots,\boldsymbol{x}_n$ into that direction, then we'll end up with our first principal component, $\boldsymbol{z}_1 = [z_{11},\dots,z_{n1}]'$
- The second principal component is the linear combination, $z_{i2} = \phi_{12}x_{i1} + \phi_{22}x_{i2} + \dots + \phi_{p2}x_{ip}$ that has maximal variance among all linear combinations that are `uncorrelated` with $z_1$
- The key difference between a regression line and a PCA line is that in the regression line we are trying to reduce the distance between the data points for the residuals but in a PCA line we're thinking about orthogonal distances instead
![[Pasted image 20231026225802.png]]
### Total Variance
- Typically when we are using PCA, we want to scale our predictors first
- When we think of the total variance, it is the sum of the variances of each of your variables that we have 
$$
\textrm{TV} = \sum_{j=1}^p \operatorname{Var}(x_j) = \sum_{j=1}^p \frac{1}{n}\sum_{i=1}^n x_{ij}^2
$$
- If we standardize these variables so that they are all have unit variance then our total variance in the data would be just the number of variables
- When we do standardize these variables, the variance explained by our $m$-th principal component is the cumulative proportion of variance explained by:
$$
\textrm{CPVE}_k = \sum_{m=1}^k\frac{V_m}{\textrm{TV}}
$$
Now, the question is how many principal components do we need to be confident that we have extracted as much of the useful data as possible?
- Scree plot
#### Scree Plot
![[Pasted image 20231026230723.png]]
### National Track Records
```
track = readr::read_csv("data/womens_track.csv")
dplyr::glimpse(track)
```
![[Pasted image 20231026231009.png]]
```
GGally::ggpairs(track[,1:7]) + theme_bw(base_size = 24) + theme(axis.text = element_blank())
```
![[Pasted image 20231026231024.png]]
- We can see that from the correlation matrix that a lot of the variables seem to be correlated with each other hence telling us that some of the information is probably encoded multiple times

#### Compute
```
track_pca = prcomp(track[,1:7], center = TRUE, scale = TRUE)
track_pca
```
![[Pasted image 20231026231210.png]]
#### Assess
```
summary(track_pca)
```
![[Pasted image 20231026231617.png]]
![[Pasted image 20231026231629.png]]
- As the scree plot would suggest, 1 PCs would be sufficient to explain most of the variability

#### Visualize
- We can now visualize the model using biplot where we plot the principal component scores and also the contribution of the original variables to the principal component
![[Pasted image 20231026231823.png]]
Form the plot here, we can interpret that in the x-axis:
- The more left the country is the faster they are in the races
To interpret the y-axis:
- We can look at the lines that are pointing outward from the centroid of the points and see that :
	- when more up, we can see that the countries are better at short distances
	- more down the countries better at long distances
#### Interpret
- PC1 measures overall magnitude
	- High positive indicate poor programs with generally slow times across the events
- PC2 measures the contrast in the program between short and long distance events
- There are several outliers in the plot, `wsamoa, cookis, dpkorea` 
	- Because PCA is computed using variance in the data and is affected by outliers, it may be better to re-run the PCA after removing these countries
### Related Problems
- You have multivariate variables $x_1, ...,x_n$ so $x_1 = (x_11,...,x_{1p})$ 
	- Find a new set of multivariate variables that are uncorrelated and explain as much variance as possible
	- Put all the variables together in one matrix and find the best matrix created with fewer variables that explains the original data
- When doing PCA, the first goal is statistical and the second is data compression
## Dimension Reduction and Clustering
### US Arrest Data
```
library(tidyverse)
data("USArrests")
glimpse(USArrests)
```
![[Pasted image 20231026233024.png]]
```
GGally::ggpairs(df)
```
![[Pasted image 20231026233035.png]]
#### PCA for US Arrests
```
pca_arrests = princomp(df)
# install.packages("remotes")
# remotes::install_github("vqv/ggbiplot")
library(ggbiplot)
p1 = ggscreeplot(pca_arrests) +
  theme_bw(base_size = 20) + labs(y = "Prop. of explained variance")
p2 = ggscreeplot(pca_arrests,type = "cev") + theme_bw(base_size = 20) + labs(y = " Cumulative prop. of explained variance")
gridExtra::grid.arrange(p1, p2, ncol=2)
```
![[Pasted image 20231026233101.png]]
#### Biplot
```
ggbiplot(pca_arrests,
         labels = rownames(USArrests),
         labels.size = 5,
         varname.size = 8) +
  theme_bw(base_size = 20) + 
  coord_cartesian(ylim = c(-2.5,2.5), xlim = c(-2.5,2.5))
```
![[Pasted image 20231026233121.png]]
#### Clustering
```
set.seed(1)
cl_arrests = kmeans(
  df, centers = 4, nstart = 10
)
p = ggbiplot(
  pca_arrests,
  groups = factor(cl_arrests$cluster),
  ellipse = TRUE) + 
  theme_bw(base_size = 20) +
  theme(legend.position = "none")
p
```
![[Pasted image 20231026233532.png]]

```
library(factoextra)
fviz_cluster(cl_arrests, data = df) + theme_classic(base_size = 24)
```
![[Pasted image 20231026233542.png]]