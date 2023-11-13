## Unsupervised Clustering
- Partitioning
- Hierarchical
![[Pasted image 20231024162409.png]]

## K-means clustering
- Choose K and then
1. Randomly assign a number from 1 to K to each observation
2. Iterate until the cluster assignments stop changing
	- For each of the K clusters, computer the cluster centroid (the mean of the cluster)
	- The kth cluster centroid is the vector of the p covariate means for the observations in the kth cluster
	- Assign each observation to the cluster whose centroid is closest
- How to do it in R
```
km <- kmeans(x,2)
km$cluster
```
![[Pasted image 20231024162749.png]]

### Hierarchical Clustering
- Produces a tree or a dendrogram
- Avoiding specifying how many clusters are appropriate by providing a partition for each $k$ obtained from cutting the tree at some level

- Tree is built in two distinct ways
- Bottom-up: Agglomerative clustering

- Top-down divisive cluster
	- All points in their own cluster

#### Distance Metrics
- Euclidean
- Manhattan

#### Code for Hierarchical Clustering
```
hc <- x |>
	dist(method = "euclidean") |>
	hclust(method = "complete")

cutree(hc, k = 2)
```

### When and Why cluster?
- Some advantages of hierarchical clustering:
	1. Don't need to know how many clusters you're after
	2. Can cut hierarchy at any level to get any number of clusters
	3. Easy to interpret hierarchy for particular
	4. Deterministic
- Some advantages k-means clustering:
	1. Can be much faster than hierarchical clustering, depending on data
	2. Nice theoretical framework
	3. Can incorporate new data and reform clusters easily