note: for parameter optimization used GridsearchCV for sklearn functions, and PSO for non-sklearn (SCAD, SQRT LASSO)


# Dataset
## Boston Housing
First, we can look at the Boston Housing Data set. This data set records various variables of a home such as the distance to a highway, crime rate of the neighborhood,
and age of the home to name a few of the variables, then also has a field indicating the value in USD of the house. To understand how these variables are correlated, we 
can observe the heatmap below showing each variable and their respective correlation coefficients. Here, Red and Blue indicate strong positive and negative correlation 
respectively.

![bhousecorr](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/bhousing_corr_plot.png)

We observe some strong correlation between input features in our data set. We can apply various regression techniques to observe what method minimizes the error in observation

# Hyperparameter Tuning
Some of the methods that are utilized for regression have hyperparameters, which dictate how the function is applied to the input data. These parameters must be tuned
when applied to our data set, as there is no "one-size-fits-all" combination of parameters for any of the regressor functions that is universally optimal.
As such, to identify what hyperparameter combination is best for application to this data set. 

The methods taken for hyperparameter tuning depend on the regressor we are tuning. In the case of various regressors from the SKLearn library, we are able to use 
an SKlearn function called GridSearchCV. This function is given a set of parameter combinations, and then iteratively evaluates each combination to identify what 
parameters achieve the lowest error. 

In some instances, such as with SCAD and Square Root LASSO regularized regression, we instead utilize Particle Swarm Optimization (PSO) for
hyperparameter tuning. PSO is a method which tries to find the optimal solution for a problem by iteratively testing combinations of hyperparameters 
and converging the minimum error rate produced by each combination of hyperparameters. So, if a set 


## Regularized Regression
