
Data cleaning:
1. fill missing data
2. smooth noisy data
3. identify or remove outliers
4. resolve inconsistencies
5. standardize/normalize data
6. fuse/merge inconsistent data


Estimating missing value?
1. default value?
2. manually filling
3. avg
4. avg over based on certain other attributes?
5. probabilistic methods? (bayesian, regression, decsion tree)
6. neural network?


Multiple Imputation by Chained Equations (MICE)

Data transformation:

1. Min-max normalization           v' = (v - min) / (max - min)
2. standardization / z-score normalization         v' = (v - v_bar) / mu_v



Noisy data:
1. binning method
2. clustering - detect and remove outliers
3. semi automated - combined human and computer
4. regression (smooth by fitting to a regression function)



Inconsistencies and redundant data should be removed

Data fusion v/s data privacy?
 obfuscate for protection:
 1. k-anonymity (generalize)
 2. make data less specific - use binning
 3. age groups, zip code groups
 4. make blobs instead of points

Data reduction:
Sampling:
1. randomized
2. stratified



Data augmentation in Machine learning:
1. rotations
2. translations
3. zooms
4. flips
5. color perturbations
6. crops
7. add noise by jittering

Generate new samples according to data distribution:
1. cluster the data (outliers will form clusters as well)
2. the size of each cluster represents its percentage in the population
3. randomize new samples – bigger clusters get more samples
4. add a small randomized value to either the mean or an existing sample
5. do this for every dimension of the chosen mean or sample
