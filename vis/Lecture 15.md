
Visual analytics system design and evaluation:
1. with human in the loop

Task #1: Classification
1. classification model: absolute or probabilistic
2. scoring a class
3. humans label the samples
4. set initial rule and refine rule to get better data samples

Task #2: Regression
1. value estimation
2. fit data to a function
3. human factor: detect outliers

Task #3: Similarity Matching
1. human factor: identify effective features

Task #4: Clustering
1. preliminary domain exploration
2. outlier detection
3. human factor: labeling, verification

Task #5: Co-occurence grouping
1. find association between entities colocated together
2. difference to clustering: in clustering it is based on the object's attributes, whereas in this it is based on objects placed together

Task #6: Profiling

Task #7: Link Prediction

Task #8: Data Reduction
1. find latent variables
2. dimension reduction
3. sampling

Task #9: Causal Modeling



Steps for Visualization Tasks:
1. Domain Problem Characterization
2. Data/operation abstraction design - map from domain/vocabulary to abstraction
3. Encoding/interaction technique design - 
	1. visual encoding - how to best show the data
	2. Interaction design - how to best support the intent the user has
	3. Algorithm design



Hypothesis Testing:
1. Question : Is method A or B better for this task?
2. Hypothesis testing frames this as a statement:
	- There is no difference in the mean time to complete the task with method A or B
	- this is the null hypothesis
	- statistical procedures reject or accept the null hypothesis


ANOVA:
1. want to find out the difference between groups of data
2. find variance in each group
3. find variance between groups
4. F = between groups / within groups, larger F means reject Null Hypothesis


F(b,w)
b = total number of groups - 1
w = total number of obs - number of groups - 1


If p < 0.05 significant
For non significant effects  use "ns" if F < 1.0 and p > 0.05 if F > 1.0

example:
F(1,9) = 0.626, ns


Post Hoc Comparison Test to determine which conditions differed significantly from one another
Ex: Scheffe Post Hoc comparisons


Between subject design

ANOVA Reporting -> **practice reporting***
Chi-square critical values


