# Project 2
In this project, we will be looking at various regularization techniques. 

## Dataset

## Background
At a high level, regularization is a method in which one is able to determine values for weights or select variables. In application,
this is an optimization problem with constraints applied to the vector of weights,  ,in a given problem. So, when a regularization method is utilized, we are 
either doing so to account for strong multi-collinearity between features in our regression, or we are accounting for the need to learn more 
parameters for our model than independent observations in the data set.


## Ridge
Ridge regularization, referred to as L2, is a form of regularization that combats some of the shortcomings of Ordinary Least Squares (OLS) 
by controlling for the size of each estimator. This is done through the minimization function:

minimize $\frac{1}{n} \cdot \sum_{i=1}^n(\text{Residual}_i)^2 + \alpha \sum_{j=1}^p \beta_j^2$

$\alpha$ is a term that specifies the strength of regularization, and generally can neither be too strong nor too weak. This parameter can be tuned based on the problem.

We need Ridge regularization to reduce the standard errors of a regression especially when there may be some form of multicollinearity in a multi-variable
regression data set. 

## LASSO
Least Absolute Shrinkage and Selection Operator (LASSO), referred to as L1, is a regularization technique which learns the weights of esimators by 
optimizing the following function:

minimize $\frac{1}{n} \cdot \sum_{i=1}^{n} (\text{Residual}_i)^2 + \alpha \sum_{j=1}^{p}|\beta_j|$

$\alpha$, similar to Ridge, functions as a strength hyperparameter which can be tuned in its applcation.


## Elastic Net
Elastic Net is a method of combining the LASSO and Ridge regression, L1 and L2, in a convex manner. So, Elastic Net is able to learn the weights for a given regressor
by minimizing the optimzation problem:

minimize $\frac{1}{n} \cdot \sum_{i=1}^n (\text{Residual}_i)^2 + \alpha (\lambda \cdot \sum_{j=1}^|\beta_j| + (1-\lambda)\cdot \sum_{j=1}^p \beta_j^2)$

$\alpha$ is again a strength hyperparameter, and is usually identified using the L1 ratio of $\frac{\lambda}{1-\lambda}$
$\lambda$ is defined to be $1 \le \lambda \le 1$

## SCAD

## Square Root LASSO



# References
 - https://ncss-wpengine.netdna-ssl.com/wp-content/themes/ncss/pdf/Procedures/NCSS/Ridge_Regression.pdf
