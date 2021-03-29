For this project, we are looking at data from a paper titled Physicochemical Properties of Protein Tertiary Structure Data Set. We are also going to explore General Additive Models and Nadaraya-Watson Kernel Density Estimation for this data set.

# Models
## General Additive Model (GAM)
A GAM is a generalization of the linear model in which values are predicted using smoothing functions of predictors, or features. These models allow us to combine advantageous 
features from generalized linear models by using smoothing functions as additive terms. 

## Nadaraya-Watson KDE (NW)
This form of kernel density estimation estimates predicted values as locally weighted averages by utilizing the Nadaraya-Watson kernel. This kernel can be see in the formulae 
below. We represent the input features as X and the dependent variable as Y. Mathematically, this kernel is represented by an estimation of the joint probability density 
function, f, and the marginal density function, f_x, as seen in the first equation. The more generealized form of this equation is then show as a summation of this proportion 
of density functions to ultimately calculate the conditional expectation of what a given value, y, is dependent on the given input features, x. 


