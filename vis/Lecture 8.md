
Box plots
1. min
2. max
3. lower quartile Q1
4. median
5. upper quartile Q3
6. Interquartile range (IQR)

Non normal distributed data give wrong box plots

- Density Plots
- Strip Plots (jittered points)
- Semitransparent v/s jittering
- Lollipop charts



**Curse of dimensionality:**
Sparseness demo:
1. with increasing number of dimensions - points get pulled apart further
2. distance becomes meaningless
3. sparseness increases
4. indexing and storage also gets expensive


Scatterplot matrix:
1. multivariate data is plotted across tiles
2. difficult to see relationships
3. biplots are one way to visualize this



Biplots:
1. Projection:
	1. define orthogonal vectors
	2. take dot product with the orthogonal vectors
	3. can cause projection ambiguity - close neighbors in projection may not be close neighbors in original higher dimensional space
	4. use first 2 PCA vectors as basis
		1. for data vectors = PCA1 . datavector,    PCA2 . datavector
		2. for dimension axes = PCA1 . dimension,   PCA2 . dimension
	5. can have projection ambiguities


Multivariate scatter plots

Subspace Voyager

Multidimensional Scaling:
1. maintains similarity relationships, prevents ambiguity
2. tries to ensure distance btw points are closely matched
3. input to MDS is a dissimilarity matrix (distance pairs in a n x n matrix).


Force directed algorithm:
1. spring like system
2. move nodes until energy is minimum




Parallel Coordinates 
- data axes are aligned in parallel side to side
- draw lines for 100 cars
- hard to see data - group into subpopulations
	- clustering
	- individual polylines - abstract away - just show clusters

Patterns in parallel coordinates
Patterns in scatter plots


Axis reordering problem:
1. how many way to order the axis:
	- n! - 7 axis - 5040 ordering
	- can already see relationships across 3 clusters - therefore no of orderings required is       n! / 3! (n-3!)



Building a mass spring model from a correlation matrix, create MDS blocks, running Travelling Salesman Problem (TSP) on the correlation nodes, and using it to order our parallel axes using TSP

- Bigger vertice means more variance...


But what if we need more attributes - how do we scale?

Multiscale Zooming - Zooming in multiple times, can slowly show us the various clusters of correlated vertices.

Bracketing and Conditioning:
1. bracketing - constraining a variables's range can increase correlation strength



Star coordinates:
1. point is represented as the vector sum of the axis coordinates





Radar Chart:
1. each start represents a single obs
2. can show outliers and commonalities nicely

disadv:
1. hard to make tradeoff decisions
2. distorts data to some extent when lines are filled in

