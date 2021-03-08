## Background
At a high level, regularization is a method in which one is able to determine values for weights or select variables. In application,
this is an optimization problem with constraints applied to the vector of weights,![Betas](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta_weights.gif),in a given problem. So, when a regularization method is utilized, we are 
doing so to account for strong multi-collinearity between features in our regression, or we are accounting for the need to learn more 
parameters for our model than independent observations in the data set. All in all, regularization as it is seen below is done to improve the predictive power
of our regression by manipulating the estimators of each model.

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
utilized with k=10 folds. The Mean Absolute Error (MAE) is finally obtained and then recorded in a table.]

We can also look at how well each regularization method can estimate beta parameters when given a usecase that particularly suits the advantages the methods posses; data with 
high multi-collinearity. We can first create the "ground-truth" beta estimators for the data set, then generate the data set based on these estimators accompanied by 
a noise function scaled by a scalar. Once the data set is created, we then run each regularization method and get the Beta estimators to identify which methods are closest to
identifying the ground truth Betas. 

## Boston Housing
First, we can look at the Boston Housing Data set. This data set records various variables of a home such as the distance to a highway, crime rate of the neighborhood,
and age of the home to name a few of the variables, then also has a field indicating the value in USD of the house. To understand how these variables are correlated, we 
can observe the heatmap below showing each variable and their respective correlation coefficients. Here, Red and Blue indicate strong positive and negative correlation 
respectively.

![bhousecorr](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/bhousing_corr_plot.png)

The strong correlation we observe between the variables in our data set indicate that we may infact need regularization! This is because regularization allows us to still 
effectively estimate parameters despite strong multi-collinearity in our data set.

So, the variables are regressed against the price to attempt to estimate housing prices in Boston. Each regularization method alongside a baseline linear model is fit to the data 
and the Mean Absolute Errors are recorded below.

| Method            | MAE       | Best Alpha | Best Lambda/Lambda Ratio |
|-------------------|-----------|------------|-------------|
| Linear Regression | $3,433.87 | N/A        | N/A         |
| Ridge             | $3,419.47 | 0.1        | N/A         |
| Lasso             | $3,421.35 | 0.05       | N/A         |
| Elastic Net       | $3,421.45 | 0.01       | 0.95        |
| Square-root Lasso | $3,483.33 | 0.96       | N/A         |

From the above data outputs, we see that after hyper parameter tuning, Ridge regularization appears to minimize the error of regression the most among the various
regularization techniques.

## Forest Fires Data set
First, we can look at the Forest Fires Data set. This data set is composed of various climate-related indices as well as meteorological data such as temperature, humidity, 
wind, and rain which are all utilized as features to predict the area burned by a given fire. To understand how these variables are correlated, we 
can observe the heatmap below showing each variable and their respective correlation coefficients. Here, Red and Blue indicate strong positive and negative correlation 
respectively.

![bhousecorr](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/fire_corr_data.png)

The strong correlation we observe between the variables in our data set indicate that we may infact need regularization! This is because regularization allows us to still 
effectively estimate parameters despite strong multi-collinearity in our data set.

So, the variables are regressed against the price to attempt to estimate housing prices in Boston. Each regularization method alongside a baseline linear model is fit to the 
data and the Mean Absolute Errors are recorded below.

| Method            | MAE       | Best Alpha | Best Lambda/Lambda Ratio |
|-------------------|-----------|------------|-------------|
| Linear Regression | 19.97 | N/A        | N/A         |
| Ridge             | 19.88 | 0.95        | N/A         |
| Lasso             | 19.66 | 0.95       | N/A         |
| Elastic Net       | 19.46 | 0.95       | 5        |
| Square-root Lasso | 19.72 | 0.95       | N/A         |
| SCAD | 18.58 | 1.0       | 0.75         |

We can tell from above that utilizing regularization techniques can notably improve our regression performance. Of all of the methods, we see that SCAD in particular
was the most effective at minimizing our error in regression.

## Randomly Generated Dataset
To further explore these regularization techniques, I have generated a data set for testing. The dataset was randomly generated using the 
numpy library as well as the toeplitz function from scipy, so as to ensure we effectively simulate multiple correlations within the data. This can be seen below.

![feat4corr](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/simdat_4feat_corr.png)

The data set has 200 samples and was made by first generating a set of 5 betas as the 
established "ground truth", then creating X and Y values based on these betas, where the Y values were generated by passing the X values as well as a
random noise function scaled by a scalar constant. Once the data set was generated, each regularization method was evaluated as done in the Boston 
Housing example above. From each iteration, we see the results below. 

| Method            | MAE       | Best Alpha | Best Lambda/Lambda Ratio |
|-------------------|-----------|------------|-------------|
| Linear Regression | 1.68 | N/A        | N/A         |
| Ridge             | 1.66 | 0.995        | N/A         |
| Lasso             | 1.50 | 0.25       | N/A         |
| Elastic Net       | 1.49 | 0.17       | 0.99        |
| Square-root Lasso | 1.49 | 16       | N/A         |
| SCAD | 1.48 | 1.0 | 0.02|

Yet another key point to observe between these regularization methods is their respective abilities to estimate parameters. As such, the data set was generated with 8 beta
parameters as "ground-truth" and then a scalar coupled with a noise function were utilized to obscure these ground truth betas. Thus, it is the task of these regularization
methods to identify these ground truth parameters.

|                   | Beta1  | Beta2 | Beta3 | Beta4 | Beta5 | L2 Norm |
|-------------------|--------|-------|-------|-------|-------|-------|
| Ground-Truth      | -1     | 6     | 9     | 3     | 0 |  |
| Linear Regression | -0.611 | 6.125 | 8.718 | 2.710 |0.0130 | 25.706 |
| Ridge             | -0.480 | 6.037 | 8.598 | 2.722 | 0.163| 25.413 |
| Lasso             | 0 | 5.389 | 8.854 | 2.485     | 0 | 25.185 |
| Elastic Net       | 0 | 5.440 | 8.811 | 2.565  | 0 | 25.145 |
| SCAD              | -0.285 | 5.954 | 8.454 | 2.880 | 0.637|24.774|
| Square-root Lasso | 0 | 5.425 | 8.825 | 2.548     | 0|25.158|

When comparing the estimated betas relative to the ground truth that we manufactured, despite LASSO performing the best in terms of MAE, it does not accurately predict Beta1 
well. Most methods actually do not end up predicting Beta1 as well as the initial linear model does. Yet despite this, the L2 Norm tells us that SCAD is the regularization
technique that is closest to the ground-truth betas, and thus the best method for this data set.

## Another Randomly Generated Dataset
This time, the data set is composed of 8 features as opposed to the 20 above. The same methodology for hyperparameter selection is followed as well as for the 
data set generation. Similar to what we have done above, we may observe each feature's correlation to eachother in the heatmap below.

![bhousecorr](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/corr_simdata_feat8.png)



| Method            | MAE       | Best Alpha | Best Lambda/Lambda Ratio |
|-------------------|-----------|------------|-------------|
| Linear Regression | 2.01 | N/A        | N/A         |
| Ridge             | 1.98 | 0.995        | N/A         |
| Lasso             | 1.76 | 0.085       | N/A         |
| Elastic Net       | 1.81 | 0.05       | 2.8        |
| Square-root Lasso | 1.74 | 5.4       | N/A         |
| SCAD | 1.67 | 3.0 | 0.03|

To examine how well the regularization methods can estimate the ground-truth betas as outlined above, the methodology from above is followed. In the table below, each beta is 
estimated by each method, and we can visually observe performances of these methods in estimating these ground-truth beta parameters.

|                   | Beta1  | Beta2 | Beta3 | Beta4 | Beta5  | Beta6 | Beta7 | Beta8  | L2 Norm |
|-------------------|--------|-------|-------|-------|--------|-------|-------|--------|--------|
| Ground-Truth      | -1     | 2     | 3     | 0     | 0      | 4     | 2     | -1     | |
| Linear Regression | -0.799 | 1.820 | 2.562 | 0.241 | -0.712 | 4.158 | 2.254 | -1.187 | 20.219| 
| Ridge             | -0.734 | 1.769 | 2.513 | 0.243 | -0.594 | 4.039 | 2.215 | -1.100 |19.806|
| Lasso             | -0.467 | 1.488 | 2.504 | 0     | -0.002 | 3.728 | 2.004 | -0.806 |18.602|
| Elastic Net       | -0.464 | 1.490 | 2.498 | 0     | 0      | 3.716 | 2.008 | -0.802 |18.581|
| SCAD              | -1.257 | 2.393 | 3.726 | 0     | 0      | 4.494 | 2.236 | -1.402 |17.869|
| Square-root Lasso | -0.0484 | 1.603 | 2.506 | 0     | 0      | 3.802 | 1.935 | -0.686 |19.596|

We can notice that both Elastic Net, SCAD, and Square-root Lasso are effective in estimating the Beta4 and Beta5 parameters to be 0. Despite this, Elastic Net 
performs notably less effectively in estimating larger Beta values, notably Beta3 and Beta6. SCAD is the most effective at estimating Beta3, however does not perform as well 
when considering the other Beta parameters. Square-root Lasso on the other hand performs the best of the regularization 
methods at estimating Beta3 as well as one of the better methods in estimating Beta7. Yet overall, we can look at the L2 Norm and the MAE to best understand what 
regularization technique performs best. From this, we can see that SCAD performed best not only in terms of the MAE but also had the smallest L2 Norm, meaning
that the parameters estimated by SCAD were closest to the ground-truth betas we manufactured.

# References
 - https://ncss-wpengine.netdna-ssl.com/wp-content/themes/ncss/pdf/Procedures/NCSS/Ridge_Regression.pdf
 - https://andrewcharlesjones.github.io/posts/2020/03/scad/
 - https://fan.princeton.edu/papers/01/penlike.pdf
 - https://statisticaloddsandends.wordpress.com/2018/07/31/the-scad-penalty/

