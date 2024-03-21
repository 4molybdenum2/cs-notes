Data reduction:
1. random sampling
2. stratified sampling

Reduce the number of attributes:
1. dimension reduction by transformation
2. dimension reduction by elimination

do both, throw away redundant superflous data, keep gist


growth of data and complexities is fast - therefore reduction techniques required

Measure of similarity:
- distance measure in high dimensional feature space

Feature space:
- dimensions which can be defined by various attributes, more features, better differentiation
- how many features do we need?
- quantize into vector - calculate similarity using distance function

Distance functions:
1. Manhattan distance: sum |a-b|
2. Euclidian distance: sqrt(sum (a-b)^2)

Cosine similarity:


s(x,y) = x.y / ||x||.||y||
	 = sum from i to n-1 (xi . yi) / sqrt(sum from i to n-1 xi^2) * sqrt(sum from i to n-1 yi^2)


Pearson's correlation: correlation similarity

Correlation distance is invariant to addition of a constant
- substracts out by construction
- correlated vectors vary in the same way
- cosine similarity is stricter

Both correlation and cosine distance is invariant to scaling (multiplication by a constant)

Correlation = Covariance(X,Y) / sqrt(Variance(X)) * sqrt(Variance(Y))

Jaccard Similarity: |A intersection B| / |A union B|


Clustering: grouping of similar items
Cluster: group of objects that are similar to each other and dissimilar from other objects at the same time
MSE: objective is to minimize MSE
can be multiple local minima, need to find one local minima, this can be difficult


K-means clustering algorithm:


1. select k
2. select k centroid randomly
3. assign points to each cluster centroid one by one by calculating euclidian distance from centroid
4. find mean of each cluster these are now centroid of cluster, and go to 3 again
5. if clustering didn't change stop


this is not always ideal
- we can assess clustering by adding up variation within each cluster and adding them up
- 



strengths:
1. efficient
2. simple to code
weakness:
1. need to k before hand, else try multiple k to choose best
2. often terminates at local optimum
3. global optimum can be found by trying multiple times and taking the best result


-> When to stop looking? Plot for multiple K and check flattening of curve - knee finding / elbow finding



**Adaptive sampling:**
1. Cluster data (outliers will also form clusters)
2. clusters are called strata
3. size of cluster represents its percentage in population
4. guides the number of samples - bigger cluster get more samples
5. eliminate highly correlated values (kms v/s miles)

**Redundancy sampling:**
1. eliminate redundant data (cluster data with small ranges e and only keep cluster centroids, store size of clusters to store importance)
2. how do we find e? -> compute histogram of distances, choose a reasonable threshold from the left

**Reservoir sampling:**


**CURE High dimensionality sampling:**




**Dimenstionality Reduction**
1. By axis rotation:
	1. SVD
	2. PCA
	3. LSA
2. By type transformation:
	1. Fourier analysis and wavelets for grids
	2. MSD
	3. Locally Linear Embedding
	4. Isomap
	5. SOM
	6. LDA


Cov(X,Y) = sum over all ((x - mean of x) * (y - mean of y)) / n-1, where n = data items
Covariance matrix

n-D dataset has n variables, make a ndimensional covarianca matrix with all pairs of variables


PCA:
1. find a coordinate system that can represent variance in data with fewest axes as possible
2. rank these axes by the amount of variance
3. drop the axes that has the least variance

Eigen values are just measures of variation

Total variation along all the PCs is additive

Best fitting line is always PC1

Eigenvalue = average of the sum of squared distances for the best fit line for PC1
Eigenvector = 1 unit long vector that represents the proportion of variables for  PC1
Loading scores = proportion of each genes
Correlation between PC and variables is known as loadings. The amount of weight a data dimension has on a principal component

PC2 = line perpendicular to PC1 without any further optimization
similarly find eigenvector for PC2 and eigen values


Number of PCs = number of variables or the number of samples
PC for each variable



- Common rule of thumb axes with eigen values > 1 should be taken
- screeplot can contain sum of squared loadings and reduce the number of variables needed o explain a dataset, by choosing a significane threshold


