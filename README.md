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

# Data Processing
Before the data is input into each model, we can utilize the PolynomialFeatures and StandardScaler functions from the SKLearn library. These functions allow us to scale
our data to be better understood by each regressor. Using polynomial features and scaling allows us to better find the potential interactions
between features that may exist in our data set. Thus, in implementation, we have chosen to create PolynomialFeatures of degree 3 from the data set and then scale the data
before processing in each model. 

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
hyperparameters having been identified through hyperparameter tuning methods outlined above. For sake of better understanding what methods we are working with in
this project, I have included descriptions of each method from my previous work below. 

## Linear Model
For purposes of understanding how our regressors would perform against a baseline, a simple linear model is utilized. This model can be thought of trivially as a
line of best-fit within the data, where each input feature has a weight, or a Beta, associated with predicting the dependent variable, housing price. 

## Regularized Regression
For applications of regularized regression, we have a variety of models in our toolkit: Ridge, LASSO, Elastic Net, Square Root LASSO, and SCAD. Noticing how much
multi-collinearity is present in the data set, these methods for regression will likely perform very competitively against the other methods. 

### Ridge
Ridge regularization, referred to as L2, is a form of regularization that combats some of the shortcomings of Ordinary Least Squares (OLS) 
by controlling for the size of each estimator. Ridge allows us to particularly combat multi-collinearity. This is done through the minimization function:

![ridge_eqn](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/ridge_eqn.gif)

![alpha](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/alpha.gif) is a term that specifies the strength of regularization, and generally can neither 
be too strong nor too weak. This parameter can be tuned based on the problem.
We can visually see what this penalty looks like in the plot below:

![RIDGEplot](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/RidgePenalty.png)

We need Ridge regularization to reduce the standard errors of a regression especially when there may be some form of multicollinearity in a multi-variable
regression data set. One thing of note is that, when observing the penalization function 
![ridgepenalty](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/ridgepenalty.gif), we see that as the estimation of a ![beta](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta.gif)
increases, the penalty on the estimation of ![beta](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta.gif) increases quadratically. Thus, larger ![beta](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta.gif) terms are penalized more harshly. 

### LASSO
Least Absolute Shrinkage and Selection Operator (LASSO), referred to as L1, is a regularization technique which learns the weights of esimators by 
optimizing the following function:

![LASSO_eqn](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/lasso_eqn.gif)

![alpha](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/alpha.gif), similar to Ridge, functions as a strength hyperparameter which can be tuned in 
its application. We can again visually see what this penalty look like in the
plot below:

![LASSOplot](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/LASSOpenalty.png)

We can see that as we encounter larger ![beta](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta.gif) values, the LASSO regularization method 
penalizes the estimation of the parameter more and more harshly. Thus, we see the 
penalization of parameters increases linearly as the parameter value increases. LASSO particularly allows us to help get more parsimonious models.

### Square Root LASSO
Square Root LASSO is a modification to the LASSO technique, where an L1 penalty is still considered, yet with an objective function of which is a square root. This can be 
represented through the minimization of the optimization function:

![sqrt_lasso](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/sqrt_lasso.gif) 

The key difference to note is that Square Root Lasso takes the square root of the mean of the squared residuals as the objective function to be minimized. The penalty term 
for this method is the same as in the LASSO technique.

### Elastic Net
Elastic Net is a method of combining the LASSO and Ridge regression, L1 and L2, in a convex manner. So, Elastic Net is able to learn the weights for a given regressor
by minimizing the optimzation problem:

![elasticneteq](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/elasticnet_eqn.gif)

![alpha](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/alpha.gif) is again a strength hyperparameter, and is usually identified using the L1 ratio of ![lambdaratio](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/lambdaratio.gif)

![lambda](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/lambda.gif) is defined to be ![inequ](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/0_lambda_1.gif)

Elastic Net is able to find a decent middle ground between Ridge and LASSO regularizations, allowing the regularization to address both the multi-collinearity that 
Ridge addresses while also enabling our model to be more parsimonious in line with LASSO. 


### SCAD
Smoothly Clipped Absolute Deviations (SCAD) is a form of regularization that attempts to further combat biases that are potentially not fully addressed by the 
methods of regularization above. SCAD attempts to find a middle ground of regularization when estimating parameters by allowing for large ![beta](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta.gif) values to be identified 
for parameters while encouraging the identification of fewer more meaningful estimators rather than many potentially less meaningful ![beta](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta.gif)s. Mathematically, this is done by minimizing the function below:

![SCADmain](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/scad.gif)

Where the penalization function is frequently represented piece-wise in terms of 
![lambda](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/lambda.gif) as:

![scadeqn](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/scad_penalty.gif)

We can see from this penalization that, as ![beta](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta.gif) increases, it may reach a point where 
![betalambda](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta_alphalamb.gif) then SCAD does not further penalize the estimation of the
given ![beta](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta.gif) parameter, thus allowing for larger ![beta](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta.gif) values when compared to other techniques. We can also see how the SCAD technique does not 
take one approach unilaterally, and instead imposes linear penalty to smaller estimations of ![beta](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta.gif) and quadratic penalty to so-called medium ![beta](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta.gif) 
estimations. Looking at the plot below, we can see the advantage of SCAD over LASSO in that SCAD "smoothly-clips" the v-shaped penalty functions that LASSO provides. 

![SCADplot](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/SCADpenalty.png)

## LOWESS Regression
Local regression is a form of regression that takes into account local points throughout the plot to identify the a curve of regression. So, when a locally weighted 
regressor makes a prediction, it does so using only points local to the input. This is done by the process of identifying local weights, which are determined through the use 
of different kernels or mathematical functions which weight a regression based on local 
subsets of the overall data set. There are a host of different kernels that can be utilized for weighting (read more about these kernels [here](https://en.wikipedia.org/wiki/Kernel_(statistics))), yet only four will be considered for this project: tricubic, 
quartic, uniform, and the Epanechnikov kernel functions.

## XGBoost
Extreme Gradient Boost, or more colloqiually known as XGBoost, is a form of Gradient Boost that is particularly effective at combatting overfitting. This is thanks to the 
ability for XGBoost to regularize parameters. Gradient Boost is a form of regression that utilizes an ensemble of decision trees to make predictions. More 
concisely, XGBoost is utilizing a forest of decision trees, similar to Random Forests, yet XGBoost improves upon RF by weighting trees differently, thereby making the 
regressor a stronger learner as it can be more sensitive to new data.

## Sequential NN
A Sequential Neural Network is a neural network, where each layer is built sequentially and the NN particularly handles sequences of data. So, each iteration of the sequence 
builds a number of neurons within the network, until finally the network is built and fit with the training data. For the purposes of this project, the model is sequentially 
built starting with 128 neurons, then 32 neurons, then 8 neurons, then finally a singular neuron for the last layer. The first three layers have a rectified linear 
activation function, while the final layer has a linear activation function. The reason this final layer is composed of one neuron is because this is the output layer, and 
is necessary since we are utilizing this NN for a regression task! The NN is then compiled with the ADAM optimizer, an adaptive version of gradient descent, and finally fit 
with the training data.

## Random Forest
Random Forests is a regression algorithm that is an expansion upon the Decision Trees algorithm. The algorithm works by creating a forest of Decision Trees that are randomly
sampled with replacement. RF then weights each Decision Tree to control for some of the overfitting issues that the Decision Tree algorithm is prone to. 


# Model Evaluation
For purposes of understanding how these regression methods perform against eachother, we apply each of them to the Boston Housing data set. In terms of how these methods 
are applied, we utilize KFold Cross Validation to ensure that we are minimizing the bias in our error statistics. 

## KFold
KFold is a form of cross validation, where we iteratively test a model to better understand its predictive power through metrics such as Mean Absolute Error (MAE). KFold works by splitting up a training dataset into "k" folds. Then, the model is trained on k-1 "folds" of the data set and validated on the the final fold within a given iteration. KFold iterations k times, so finally the performance metrics are identified by taking the mean of the metrics from each iteration. It is particularly worth noting that each fold is equal in length, and a new fold is picked in each iteration of KFold as the validation set. 

An example of this algorithm can be seen in the image below, where an instance of KFold is run for k=4. 
![kfold](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/kfold_example.png)

KFold is a critical step in the process of evaluating various regressors, because it allows us to obtain less-biased estimations of error for a given regressor. This is thanks to how KFold "shuffles" the data into unique folds and then iteratively trains and tests against different combinations of these folds. This allows the estimated error to account for particularly "good" or "bad" train and test splits within the data that may come about from testing.

Without KFold, we could be getting biased evaluation metrics and thus choose a potentially overfit or underfit model for our regression problem. Thus, it is very important that we do not forget to utilize KFold in evaluating our variety of regressors. 


# Results
Following the methodologies outlined above, we finally can apply each model to the data set and observe the optimal hyperparameters chosen for each model as well as the
lowest MAE observed for each model. 

## Best Parameters

### Kernel Regression
| LOWESS Kernel     | MAE      | Tau |
|-------------------|----------|-------|
| Tricubic | $2251.05 | 0.3    | 
| Quartic | $2251.07 | 0.3    | 
| Uniform | $2250.89 | 0.3    | 
| Epanechnikov | $2250.87 | 0.3  |

### Regularized Regression

| Method            | MAE      | Alpha | Lambda |
|-------------------|----------|-------|--------|
| Linear Regression | $3491.85 | N/A   | N/A    |
| Ridge             | $2161.45 | 8     | N/A    |
| LASSO             | $2151.37 | 0.015 | N/A    |
| Elastic Net       | $2125.39 | 0.015 | 0.25   |
| Square Root LASSO | $2138.40 | 0.01  | 10    |
| SCAD              |$2407.42 | 0.02  | 1.5    |

### XGBoost
| Method     | MAE      | Reg Lambda | Max Depth| Alpha | Gamma| n estimators|
|-------------------|----------|-------|-------|-------|-------|----------|
| XGBoost | $2384.10 | 20 | 3 | 1 | 10 | 100 |

### Random Forests
| Method     | MAE      | Bootstrap | Max Depth| Max Features | Min Samples Leaf | Min Samples Split| n estimators| 
|-------------------|----------|-------|-------|-------|-------|-------|-------|
|Random Forests | $2815.32 | True | 110 | 3 | 4 | 8 | 100 |



## Error Rates Observed
| Method     | MAE      |
|-------------------|----------|
|Sequential NN| $2210.59|
| XGBoost | $2384.10 |
| Epanechnikov | $2250.87 | 
| Uniform | $2250.89 |
| Quartic | $2251.07 | 
| Tricubic | $2251.05 |
|Random Forests | $2815.32 |
| Linear Regression | $3491.85 |
| Ridge             | $2161.45 |
| LASSO             | $2151.37 |
| **Elastic Net**     | $2125.39 |
| Square Root LASSO | $2138.40 |
| SCAD              |$2407.42 | 

From all of the above regressor results, we see that in terms of the minimization of the MAE, the Elastic Net algorithm of regularized regression performed
best when applied to this data set after the polynomial featuring and scaling of the data. This makes sense given the high levels of multi-collinearity that we observe
in the initial graphic above. Always remember to try a variety of methods in your toolkit to your data, as there is never a one-size-fits-all solution!
