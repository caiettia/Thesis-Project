# Project 2
In this project, we will be looking at various regularization techniques, and how they can affect the efficiency of prediction for regression.

## Background
At a high level, regularization is a method in which one is able to determine values for weights or select variables. In application,
this is an optimization problem with constraints applied to the vector of weights,![Betas](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta_weights.gif),in a given problem. So, when a regularization method is utilized, we are 
either doing so to account for strong multi-collinearity between features in our regression, or we are accounting for the need to learn more 
parameters for our model than independent observations in the data set. All in all, regularization as it is seen below is done to improve the predictive power
of our regression by manipulating the parameters of each model.

## Ridge
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

## LASSO
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
 
## Elastic Net
Elastic Net is a method of combining the LASSO and Ridge regression, L1 and L2, in a convex manner. So, Elastic Net is able to learn the weights for a given regressor
by minimizing the optimzation problem:

![elasticneteq](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/elasticnet_eqn.gif)

![alpha](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/alpha.gif) is again a strength hyperparameter, and is usually identified using the L1 ratio of ![lambdaratio](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/lambdaratio.gif)

![lambda](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/lambda.gif) is defined to be ![inequ](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/0_lambda_1.gif)

Elastic Net is able to find a decent middle ground between Ridge and LASSO regularizations, allowing the regularization to address both the multi-collinearity that 
Ridge addresses while also enabling our model to be more parsimonious in line with LASSO. 


## SCAD
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

To better understand the hyperparameters for SCAD, lambda and alpha, can affect the parameter estimations, various combinations of parameters are observed in the plot below.

![SCADcombos](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/SCAD_explore_diff_param_combos.png)

Now, when considering all of the plots above, we can see that as the alpha parameter increases, the angle of the "v" shape increases. When considering the lambda parameter,
we see that as lambda increases the scale of the y-axis increases as well. 

## Square Root LASSO
Square Root LASSO is a modification to the LASSO technique, where an L1 penalty is still considered, yet with an objective function of which is a square root. This can be 
represented through the minimization of the optimization function:

![sqrt_lasso](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/sqrt_lasso.gif)


# Observations
It is worth noting that, the main difference between LASSO and Ridge regression is namely the order of the penalty function, with LASSO having a first order penalty and
Ridge having a second order penalty function. We can further visually explore this relationship in the graphic below:

# Comparing Regularization Methods
For purposes of better understanding each regularization method beyond the theoretical level, we can apply each method to some data sets and see how they perform. To 
understand and compare methods, we use a simple linear regression with no regularization as the base line. Following this, we are able to then utilize SKLearn's GridSearchCV 
method to tune the hyper parameters for each regularization method. GridSearchCV does this by fitting each model with different combinations of parameters and then scoring 
them to identify which combination of parameters performs best on the data set. Once identified, these parameters are provided to each model, and KFold Cross Validation is 
utilized with k=10 folds. The Mean Absolute Error (MAE) is finally obtained and then recorded in a table.

## Boston Housing
First, we can look at the Boston Housing Data set. This data set records various variables of a home such as the distance to a highway, crime rate of the neighborhood,
and age of the home to name a few of the variables, then also has a field indicating the value in USD of the house. These variables are regressed against the price to attempt 
to estimate housing prices in Boston. Each regularization method alongside a baseline linear model is fit to the data and the Mean Absolute Errors are recorded below.

| Method            | MAE       | Best Alpha | Best Lambda/Lambda Ratio |
|-------------------|-----------|------------|-------------|
| Linear Regression | $3,433.87 | N/A        | N/A         |
| Ridge             | $3,419.47 | 0.1        | N/A         |
| Lasso             | $3,421.35 | 0.05       | N/A         |
| Elastic Net       | $3,421.45 | 0.01       | 0.95        |
| Square-root Lasso | $3,483.33 | 0.96       | N/A         |

## Randomly Generated Dataset
To further explore these regularization techniques, I have generated a data set for testing. The dataset was randomly generated using the 
numpy library as well as the toeplitz function from scipy. The data set has 150 samples and was made by first generating a set of betas as the 
established "ground truth", then creating X and Y values based on these betas, where the Y values were generated by passing the X values as well as a
random noise function scaled by a scalar constant. Once the data set was generated, each regularization method was evaluated as done in the Boston 
Housing example above. From each iteration, we see the results below. 

| Method            | MAE       | Best Alpha | Best Lambda/Lambda Ratio |
|-------------------|-----------|------------|-------------|
| Linear Regression | 1.86 | N/A        | N/A         |
| Ridge             | 1.86 | 0.95        | N/A         |
| Lasso             | 1.82 | 0.05       | N/A         |
| Elastic Net       | 1.82 | 0.05       | 0.95        |
| Square-root Lasso | 1.80 | 5.4       | N/A         |


# References
 - https://ncss-wpengine.netdna-ssl.com/wp-content/themes/ncss/pdf/Procedures/NCSS/Ridge_Regression.pdf
 - https://andrewcharlesjones.github.io/posts/2020/03/scad/
 - https://fan.princeton.edu/papers/01/penlike.pdf
 - https://statisticaloddsandends.wordpress.com/2018/07/31/the-scad-penalty/

