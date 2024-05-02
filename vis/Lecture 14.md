Time varying and streaming data:

Temporal relationships are difficult to discern:
1. events take place across spatially disjoint locations
2. temporal ordering can be hard to figure out
3. because it can be expressed in relative frame

Different types of time series data:
1. discrete v/s interval
2. linear v/s cyclic
3. ordinal v/s continuos
4. ordered v/s branching v/s time with multiple perspectives

ThemeRiver
1. River widens or narrows according to changes in collective strength of selected themes in underlying docs
2. centered on y axis
3. can take negative data

StreamGraphs

Stacked Area Charts

Medical Data:
- time signals
- Lifelines - patient centric
- Lifeline2 - pattern centric
	- goals:
		- brings out temporal categorical data patterns
		- categorical event data such as diagnosis, treatments, complaints
	- features:
		- allows users to manipulate multiple resources simultaneously
		- understand relative temporal relationship between records
		- 3 operators: align, rank and filter
		- temporal summaries allows multiple grouped records to be compared
		- for example, quantity of different medicine given to a patient can be seen as a streamgraph



Cyclic Patterns:
1. time data are often cyclin (periodic) and can be represented using cycles and spikes


OculusInfo GeoTime:
1. X,Y,T
2. T represents time on X,Y coordinate



Streaming Data: Time series data with no end
1. transaction streams
2. web click streams
3. tracing data / network streams
4. social streams - twitter

Constraints:
1. one pass
2. data can't be archived due to the size of data
3. can be processed only once


Concept drift: is a shift in the statistical properties of a target variable, or what a model is trying to predict, over time
Concept evolution: 
feature evolution
1. data may evolve over time
2. various statistical attributes, such as correlation between attributes and class labels, cluster distributions may change over time


CluStream is an algorithm that can deal with clustering changes over time
- online micro-clustering - process stream in real time - maintain micro-clusters of data
- offline macro-clustering - further summarizes results of clustering, provides users with more concise understanding of the clustering over different time horizons and levels of temporal granularity


Micro-clustering algorithm:
1. k m-clusters
2. new point needs to be assigned or new cluster needs to be created
3. determine distance of the point from the centroids of the m-clusters
4. assign point if it falls in range
5. else create new cluster and remove 1 to free memory availability
6. either delete old cluster or merge cluster
7. if cluster is stale delete or else start merging


store microcluster analytics periodically
- at varying levels of granularity
- recent snapshots have lower level of granularity than older snapshots


Standard pairwise distance:

shortcomings;
1. designed for time series of equal length
2. cannot address distortions on temporal (contextual) attributes

Dynamic Time warping distance:
**constaints**
1. no skipping of beginning or end of the sequence
2. no jumps
3. monotonicity - can't go back in time


- can better accomodate local mismatches
-  The first index from the first sequence must be matched with the first index from the other sequence (but it does not have to be its only match)
- The last index from the first sequence must be matched with the last index from the other sequence (but it does not have to be its only match)
- The mapping of the indices from the first sequence to indices from the other sequence must be monotonically increasing, and vice versa, i.e. if 𝑗 > 𝑖 are indices from the first sequence, then there must not be two indices 𝑙 > 𝑘  in the other sequence, such that index 𝑖  is matched with index 𝑙  and index 𝑗  is matched with index 𝑘 and vice versa
- DTW - find the minimum cost path using dynamic programming


Time Cube



