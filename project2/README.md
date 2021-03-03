# Project 2
In this project, we will be looking at various regularization techniques. 

## Background
At a high level, regularization is a method in which one is able to determine values for weights or select variables. In application,
this is an optimization problem with constraints applied to the vector of weights,![Betas](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/beta_weights.gif),in a given problem. So, when a regularization method is utilized, we are 
either doing so to account for strong multi-collinearity between features in our regression, or we are accounting for the need to learn more 
parameters for our model than independent observations in the data set. All in all, regularization as it is seen below is done to improve the predictive power
of our regression by manipulating the parameters of each model.

## Ridge
Ridge regularization, referred to as L2, is a form of regularization that combats some of the shortcomings of Ordinary Least Squares (OLS) 
by controlling for the size of each estimator. Ridge allows us to particularly combat multi-collinearity. This is done through the minimization function:

minimize ![ridge_eqn](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/ridge_eqn.gif)

$\alpha$ is a term that specifies the strength of regularization, and generally can neither be too strong nor too weak. This parameter can be tuned based on the problem.
We can visually see what this penalty looks like in the plot below:

![RIDGEplot](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/RidgePenalty.png)

We need Ridge regularization to reduce the standard errors of a regression especially when there may be some form of multicollinearity in a multi-variable
regression data set. One thing of note is that, when observing the penalization function $\alpha \sum_{j=1}^p \beta_j^2$, we see that as the estimation of a $\beta$
increases, the penalty on the estimation of $\beta$ increases quadratically. Thus, larger $\beta$ terms are penalized more harshly. 

## LASSO
Least Absolute Shrinkage and Selection Operator (LASSO), referred to as L1, is a regularization technique which learns the weights of esimators by 
optimizing the following function:

minimize $\frac{1}{n} \cdot \sum_{i=1}^{n} (\text{Residual}_i)^2 + \alpha \sum_{j=1}^{p}|\beta_j|$

$\alpha$, similar to Ridge, functions as a strength hyperparameter which can be tuned in its application. We can again visually see what this penalty look like in the
plot below:

![LASSOplot](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/LASSOpenalty.png)

We can see that as we encounter larger $\beta$ values, the LASSO regularization method penalizes the estimation of the parameter more and more harshly. Thus, we see the 
penalization of parameters increases linearly as the parameter value increases. LASSO particularly allows us to help get more parsimonious models.
 
## Elastic Net
Elastic Net is a method of combining the LASSO and Ridge regression, L1 and L2, in a convex manner. So, Elastic Net is able to learn the weights for a given regressor
by minimizing the optimzation problem:

minimize $\frac{1}{n} \cdot \sum_{i=1}^n (\text{Residual}_i)^2 + \alpha (\lambda \cdot \sum_{j=1}^|\beta_j| + (1-\lambda)\cdot \sum_{j=1}^p \beta_j^2)$

$\alpha$ is again a strength hyperparameter, and is usually identified using the L1 ratio of $\frac{\lambda}{1-\lambda}$
$\lambda$ is defined to be $0 \le \lambda \le 1$

Elastic Net is able to find a decent middle ground between Ridge and LASSO regularizations, allowing the regularization to address both the multi-collinearity that 
Ridge addresses while also enabling our model to be more parsimonious in line with LASSO. 


## SCAD
Smoothly Clipped Absolute Deviations (SCAD) is a form of regularization that attempts to further combat biases that are potentially not fully addressed by the 
methods of regularization above. SCAD attempts to find a middle ground of regularization when estimating parameters by allowing for large $\beta$ values to be identified 
for parameters while encouraging the identification of fewer more meaningful estimators rather than many potentially less meaningful $\beta$s. Mathematically, this is done by minimizing the function below:

minimize $\frac{1}{2} \cdot ||y - X \beta||_2^2 + p(\beta)$

Where the penalization function $p(\beta)$ is frequently represented piece-wise in terms of $\lambda$ as:

$\lambda$ if $|\beta| \le \lambda$
$\frac{(\alpha \lambda - \beta)}{(\alpha-1)}$ if $\lambda < |\beta| \le \alpha \labmda$
$0$ if $|\beta| > \alpha \lambda$

We can see from this penalization that, as $\beta$ increases, it may reach a point where $|\beta| > \alpha \lambda$ then SCAD does not further penalize the estimation of the
given $\beta$ parameter, thus allowing for larger $\beta$ values when compared to other techniques. We can also see how the SCAD technique does not take one approach unilaterally, and instead imposes linear penalty to smaller estimations of $\beta$ and quadratic penalty to so-called medium $\beta$ estimations. Looking at the plot below, we can see the advantage of SCAD over LASSO in that SCAD "smoothly-clips" the v-shaped penalty functions that LASSO provides. 

![SCADplot](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/SCADpenalty.png)

To better understand the hyperparameters for SCAD, lambda and alpha, can affect the parameter estimations, various combinations of parameters are observed in the plot below.

![SCADcombos](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/SCAD_explore_diff_param_combos.png)

Now, when considering all of the plots above, we can see that as the alpha parameter increases, the angle of the "v" shape increases. When considering the lambda parameter,
we see that as lambda increases the scale of the y-axis increases as well. 

## Square Root LASSO
Square Root LASSO is a modification to the LASSO technique, where an L1 penalty is still considered, yet with an objective function of which is a square root. This can be represented through the minimization of the optimization function:

minimize $\sqrt{ \frac{1}{n} \sum_{i=1}^n (y_i - y_i^{\hat})^2} + \lambda \sum_{i=1}^p |\beta_i|$


# Observations
It is worth noting that, the main difference between LASSO and Ridge regression is namely the order of the penalty function, with LASSO having a first order penalty and
Ridge having a second order penalty function. We can further visually explore this relationship in the graphic below:

![RIDGEplot](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project2/l1_vs_l2_example.png)


# References
 - https://ncss-wpengine.netdna-ssl.com/wp-content/themes/ncss/pdf/Procedures/NCSS/Ridge_Regression.pdf
 - https://andrewcharlesjones.github.io/posts/2020/03/scad/
 - https://fan.princeton.edu/papers/01/penlike.pdf
 - https://statisticaloddsandends.wordpress.com/2018/07/31/the-scad-penalty/

