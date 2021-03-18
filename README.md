For this project, we will be looking at a variety of the regression methods we have covered thus far and how they perform when applied to a real data set. For our evaluation
we will start by first identfiying what features are statistically significant for prediction. Then, we will tune the hyperparameters of our various models through either
GridSearchCV or Particle Swarm Optimization (PSO) where applicable. Finally, we will look at the Mean Absolute Errors (MAE) produced by each model to understand what 
method of regression is most applicable to our data set.

# Dataset
## Boston Housing
First, we can look at the Boston Housing Data set. This data set records various variables of a home such as the distance to a highway, crime rate of the neighborhood,
and age of the home to name a few of the variables, then also has a field indicating the value in USD of the house. To understand how these variables are correlated, we 
can observe the heatmap below showing each variable and their respective correlation coefficients. Here, Red and Blue indicate strong positive and negative correlation 
respectively.

![bhousecorr](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/bhousing_corr_plot.png)

We observe some strong correlation between input features in our data set. We can apply various regression techniques to observe what method minimizes the error in 
observation, yet this strong multicollinearity between features indicates that regularization methods will be important in minimizing the Mean Absolute Error (MAE) of
our regression.

# Variable Selection
For identifying what variables are statistically significant for regression, we may utilize Stepwise Selection. This is a method where a simple linear model is fit with one
feature from the data, and iteratively variables are added to the model and evaluated. Stepwise utilizes an F-test for variable selection, identifying based on a p-value
with a confidence score usually of 5%, or alpha = 0.05, if a variable is statistically significant. If the p-value < 0.05, then we know that this variable is statistically
significant in predicting the dependent variable. Thus, this method is applied to the features in our data set and the variables determined to be statistically significant
are shown in the table below.

| Variable | Variable Explanation                                 | p-value   | Keep or Drop |
|----------|------------------------------------------------------|-----------|--------------|
| lstat    | % lower status of the population                     | 3.731e-89 | keep |
| rooms    | average number of rooms per dwelling                 | 3.031e-27 | keep |
| ptratio  | pupil-teacher ratio by town                          | 3.436e-14 | keep |
| distance | weighted distances to five Boston employment centers | 9.735e-6  | keep |
| nox      | nitric oxides concentration (parts per 10 million)   | 2.782e-8  | keep |
| older      | proportion of owner-occupied units built prior to 1940.   | 0.661  | drop |
| industrial      | proportion of non-retail business acres per town   | 0.550  | drop |
| residential      | proportion of residential land zoned for lots over 25,000 sq ft.    | 0.011  | keep |
| highway     | index of accessibility to radial highways | 0.0153  | keep |
| tax      | full-value property-tax rate per $10,000   | 7.576e-5  | keep |
| crime      | per capita crime rate by town   | 0.004 | keep |

From our above stepwise regression, we can see that we only end up excluding two features from the data set based on an alpha of 0.05, or 5% confidence rate. This tells us
that the proportion of owner-occupied units built prior to 1940 and the proportion of non-retail business acres per town are both not statistically significant in
explaining the median value of owner-occupied homes. Having identified these rest of the variables as statistically significant for our regression, we may proceed by
excluding the two features that are not significant (older and industrial). 

Why this is important can be easily seen through analyzing the error from a linear model. If we include all of the features from the data set, a simple linear model
produces a Mean Absolute Error (MAE) of $3491.85. If we exclude the two variables outlined above, we can refit our data to the linear model and find our MAE to be 
$3434.79. This notable drop in the error is thanks to the exclusion of variables that are not statistically significant to predicting the median home price. Thus, only the
statistically significant variables are included for this project.

# Hyperparameter Tuning
Some of the methods that are utilized for regression have hyperparameters, which dictate how the function is applied to the input data. These parameters must be tuned
when applied to our data set, as there is no "one-size-fits-all" combination of parameters for any of the regressor functions that is universally optimal.
As such, to identify what hyperparameter combination is best for application to this data set. 

The methods taken for hyperparameter tuning depend on the regressor we are tuning. In the case of various regressors from the SKLearn library, we are able to use 
an SKlearn function called GridSearchCV. This function is given a set of parameter combinations, and then iteratively evaluates each combination to identify what 
parameters achieve the lowest error. 

In some instances, such as with SCAD and Square Root LASSO regularized regression, we instead utilize Particle Swarm Optimization (PSO) for
hyperparameter tuning. PSO is a method which tries to find the optimal solution for a problem by iteratively testing combinations of hyperparameters 
and converging the minimum error rate produced by each combination of hyperparameters. Visually, we can see how this works in the 3-dimensional plot below. The function
space is the blue/green shaded region.

![RIDGEplot](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/midtermproject/pso_example.png)

In this gif, we see that each frame is an iteration of the PSO algorithm. As each iteration progresses, the algorithm hones in on the minimum of the function; the optimal
solution. With these methods, we are able to tune the hyperparameters and achieve the best possible minimization of error for each function.



# Models
We can now look at the various models from our toolkit and apply each model to the real data set. From application, we will observe the MAE as well as the optimal 
hyperparameters having been identified through hyperparameter tuning methods outlined above. These values will be recorded in the table below.

## Linear Model
For purposes of understanding how our regressors would perform against a baseline, a simple linear model is utilized. This model can be thought of trivially as a
line of best-fit within the data, where each input feature has a weight, or a Beta, associated with predicting the dependent variable, housing price. 

## Regularized Regression
For applications of regularized regression, we have a variety of models in our toolkit: Ridge, LASSO, Elastic Net, Square Root LASSO, and SCAD. Noticing how much
multi-collinearity is present in the data set, these methods for regression will likely perform very competitively against the other methods. 







# Results
Before dropping the variables.
| Method            | MSE      | Alpha | Lambda |
|-------------------|----------|-------|--------|
| Linear Regression | $3491.85 | N/A   | N/A    |
| Ridge             | $2161.45 | 8     | N/A    |
| LASSO             | $2151.37 | 0.015 | N/A    |
| Elastic Net       | $2125.39 | 0.015 | 0.25   |
| Square Root LASSO | $3130.33 | 0.96  | N/A    |
| SCAD              | $2540.32 | 0.02  | 1.5    |

After dropping the variables.
| Method            | MSE      | Alpha | Lambda |
|-------------------|----------|-------|--------|
| Linear Regression | $3434.79 | N/A   | N/A    |
| Ridge             | $2323.63 | 20     | N/A    |
| LASSO             | $2254.95 | 0.015 | N/A    |
| Elastic Net       | $2246.08 | 0.015 | 0.85   |
| Square Root LASSO | $3524.91 | 0.56  | N/A    |
| SCAD              | $2407.42 | 0.05  | 1.1    |

