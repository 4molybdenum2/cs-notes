
Cluster Analysis of Numerical Data:
When to cluster?
- data summarization
- customer segmentation
- social network analysis
- precursor to other analysis


1. Attribute Selection using Entropy
	1. entropy measures lack of order
	2. start with all attributes and compute distance entropy
	3. greedily eliminate attributes that reduce entropy most
	4. stop when entropy no longer 
2. Hierarchical Clustering
	1. In data mining and statistics, hierarchical clustering (also called hierarchical cluster analysis or HCA) is a method of cluster analysis that seeks to build a hierarchy of clusters. Strategies for hierarchical clustering generally fall into two categories: **Agglomerative**: This is a "bottom-up" approach: Each observation starts in its own cluster, and pairs of clusters are merged as one moves up the hierarchy. **Divisive**: This is a "top-down" approach: All observations start in one cluster, and splits are performed recursively as one moves down the hierarchy.
	2. Agglomerative Clustering
		1. Agglomerative Merge algorithm
		2. **How to merge?**
			1. Single linkage
			2. Complete linkage
			3. Group Average linkage
			4. Closest centroid, variance minimization, ward's method
		3. **Centroid based methods merge large clusters**
			1. **single linkage methods can chain closely related points** to discover clusters of arbitrary shape. susceptible to noise
			2. **complete linkage tends to create *spherical* clusters** with similar diameter. gives too much importance to data points on the fringe. no way to undo mistake. will split up large odd shaped clusters into smaller spherical shapes
			3. group average and wards method are more robust to noise due to use of multiple linkages
		4. **DBSCAN** - Density Based Spatial Clustering of applications with noise, groups together points that are close to each other based on a density criterion
			1. - A point p is a _core point_ if at least minPts points are within distance ε of it (including p).
				- A point q is _directly reachable_ from p if point q is within distance ε from core point p. Points are only said to be directly reachable from core points.
				- A point q is _reachable_ from p if there is a path _p_1, ..., _pn_ with _p_1 = _p_ and _pn_ = _q_, where each _p__i_+1 is directly reachable from pi. Note that this implies that the initial point and all points on the path must be core points, with the possible exception of q.
				- All points not reachable from any other point are _outliers_ or _noise points_.
		1. **Mahanobilis distance:** multivariate generilization of the square of standard score of how far away a point is from the mean of a distribution - it is unitless and scale-invariant and takes into relation the correlations between data points.
3. Probabilistic Clustering:
	1. better for point distributions
		1. need probabilistic algorithm
		2. **overlapping clusters possible**
			1. Expectation - Maximization
	2. EM algorithm:
		1. Expectation - assign points to cluster and find probability of a point belonging to a cluster
		2. Maximization step: estimate model params (mean?)
4. Linear Discriminant Analysis (LDA)
	1. **requires class labels unlike PCA**
	2. maximize separability among known categories
		1. maximize interclass variance
		2. minimize intra class variance
		3. find a basis that maximally separates the classes
	3. create new axis
		1. maximize distance between two means after the data is project onto the new axis.
		2. minimize the variation within each category
		3. Fisher criterion
5. T-SNE (t-distributed stochastic neighbor embedding)
	1. Find similiraity by plotting points v/s a current point by plotting on a normal curve. scale similarities so that they add up to 1.
	2. project data randomly on number line and calculate similiarity scores
	3. instead use a t distribution for similarity
	4. make the t dist matrix look like the normal dist similarity matrix
	5. t distribution is used so that the clusters dont clump up in the middle
	6. **shortcomings:**
		1. **doesn't preserve global data structure**
			1. **only within cluster similarities are preserved**
			2. **between cluster similarities are not guaranteed**
			3. **more recently U-MAP is used**
6. Reduction via NN:
	1. A latent space, also known as a latent feature space or embedding space, **is an embedding of a set of items within a manifold in which items resembling each other are positioned closer to one another.**
	2. latent space allow easy interpolation
	3. move between samples in latent space and reconstruct novel instances using decoder
	4. not easily possible using other non-linear layout like MDS and T-SNE
7. Deep clustering:
	1. linearizes non linear data manifolds in high D space, useful for computer vision tasks
	
Cluster Analysis of Categorical data
1. TF-IDF (Bag of Words) similarity
2. SVD: decomposes a matrix into U, D and V matrices where U are the eigen vectors of CC^T, V are the eigen vectors of C^TC and D is a diagonal matrix of sqrt(lambda_k) where lambda_k are the eigenvalues of CC^T, k= Rank(C)
3. Latent Semantic Analysis (Topic similarity)
	1. create occurence matrix
	2. use svd to reduce rows
	3. U -> rows (word matrix/ term-concept matrix), D -> Topic strength, V^T -> columns (concept-document matrix for topic)
	4. keep most significant rows from D
	5. how many concepts to use? -> use elbow method in scree plot -> we get a k-D concept space shared by both terms and documents
	6. project k-D concept space into 2-D and visualize as a map
	7. can cluster the map
	8. can mark clusters using terms
	9. **LSA disadvantages:**
		1. **assumes gaussian distribution and frobenius norm -> may not fit all problems**
		2. **cannot handle polysemy -> need LDA for this**
		3. **depends heavily on SVD -> computationally expensive, slow in online document addition**
	10. correspondence analysis (CA) for categorical values : similar to PCA but for categorical values
	11. Chi-square test
		1. Correspondence analysis of smokers and non smokers in a company
		2. create contigency table (cross tabulation contains counts of number of observations in each category)
		3. create prob dist table
		4. find df = (r-1)(c-1)
		CA:
			1. create concept matrix
			2. orthogonalize CC^T and C^TC, find eigen vectors and plot
			3. this will show us the relation between various categories in rows and columns in a biplot
		1. Multiple Correspondence analysis
	13. Gartner Magnetic Quadrant
		1. culmination of research in a specific market providing a wide angle view of relative position of market competitors.
		2. Y-axis - ability to execute
		3. X-axis - completeness of vision
			1. Niche players (0,0)
			2. Visionaries (1,0)
			3. Leaders (1,1)
			4. Challengers (0,1)
