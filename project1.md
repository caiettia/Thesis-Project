# Comparing Regressors
We are given the Boston Housing Dataset, a dataset surrounding the prices of homes in Boston and various datum surrounding them. 

We are going to be looking specifically at 

## Support Vector Regression
For purposes of understanding what regressor would be best applicable for our dataset, we can explore the various kernels for SVR. More specifically,
we will look at the linear, polynomial, and radial basis function (rbf) kernels for SVR. Following KFold with k=10 folds, we find that the MAEs yielded are as follows: 
linear $4,434.24, polynomial $36,017.88, and rbf $4,124,47.

## XGBoost
Following k=10 KFold, an MAE of $4,167.28 is observed.

## Linear Model
Following k=10 KFold, an MAE of $4,424.53 is observed.

## LOWESS
Following k=10 KFold, an MAE of $3,799.25 is Quartic observed.
Following k=10 KFold, an MAE of $3,779.39 is Epanechnikov observed.
Following k=10 KFold, an MAE of $3,795.95 is Tricubic observed.

## Sequential NN
Following k=10 KFold, an MAE of $3,915.13 is observed.

