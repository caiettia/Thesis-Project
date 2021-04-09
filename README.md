For this project, we are looking at data from a paper titled Physicochemical Properties of Protein Tertiary Structure Data Set. We are also going to explore General Additive Models and Nadaraya-Watson Kernel Density Estimation for this data set.

# Dataset
This data set is composed of 9 features, labeled f(1) to f(9), which are utilized to predict the RMSD variable. These variables are associated with the paper Physicochemical 
Properties of Protein Tertiary Structure that you may read more about [here](https://learn-us-east-1-prod-fleet01-xythos.content.blackboardcdn.com/blackboard.learn.xythos.prod/571910f3bd595/4039444?X-Blackboard-Expiration=1617073200000&X-Blackboard-Signature=uRjHNQGWBUh3FV6EEyGE3fiTr6Jwqtv5q9adF6uUun8%3D&X-Blackboard-Client-Id=100852&response-cache-control=private%2C%20max-age%3D21600&response-content-disposition=inline%3B%20filename%2A%3DUTF-8%27%2740_IJMECE%25281%2529.pdf&response-content-type=application%2Fpdf&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20210329T210000Z&X-Amz-SignedHeaders=host&X-Amz-Expires=21600&X-Amz-Credential=AKIAYDKQORRYTKBSBE4S%2F20210329%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=06c80732ca5ddb2cc59ea385595a943fb50c97e51e7eda4b4bb7cf0cf1dbed59). As per the paper, it utilized the dataset from the [UCI Machine Learning repository](http://archive.ics.uci.edu/ml/datasets/Physicochemical+Properties+of+Protein+Tertiary+Structure) to 
explore the performance of various regression models for the prediction of protein tertiary structures by modelling physiochemical properties. With this, each variable 
in the dataset is listed in the table below alongside what said variable entails.

| Variable    |   Description   |
|------|-----|
|RMSD|Size of the Residue|
|f(1) |Total surface area |
|f(2) | Non polar exposed area|
|f(3) | Fractional area of exposed non polar residue|
|f(4) | Fractional area of exposed non polar part of residue|
|f(5) | Molecular mass weighted exposed area|
|f(6) | Average deviation from standard exposed area of residue|
|f(7) | Euclidian distance|
|f(8) | Secondary structure penalty|
|f(9) | Spacial Distribution constrains (N, K value)|

# Models
## General Additive Model (GAM)
A GAM is a generalization of the linear model in which values are predicted using smoothing functions of predictors, or features. These models allow us to combine advantageous 
features from generalized linear models by using smoothing functions as additive terms. 

## Nadaraya-Watson KDE (NW)
This form of kernel density estimation estimates predicted values as locally weighted averages by utilizing the Nadaraya-Watson kernel. This kernel can be see in the formulae 
below. We represent the input features as X and the dependent variable as Y. Mathematically, this kernel is represented by an estimation of the joint probability density 
function, f, and the marginal density function, f_x, as seen in the first equation. The more generealized form of this equation is then show as a summation of this proportion 
of density functions to ultimately calculate the conditional expectation of what a given value, y, is dependent on the given input features, x. 

![eqn1](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project_4/estimate_jointpdf.gif)

![eqn2](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project_4/nwkde_expectation.gif)

# Evaluation
Each model is applied to the data set and the f(x) variables are utilized as features to predict the dependent variable, RMSD. To evaluate each model, the performance metrics 
of R-squared and Root Mean Square Error (RMSE) are utilized. R-squared, or the coefficient of determination, will tell us how well the data is fit to the regression line.
Thus, a higher R-squared is preferred, and values can range from 0 to 1. The RMSE is a statistic to show the standard deviation of the residual values; the difference between 
predicted values and the ground-truth values each model has attempted to predict. For purposes of this application, the SKLearn library is utilized to calculate both of these 
metrics. 

To ensure that we are getting an unbiased evaluation of both of our models, KFold crossvalidation is utilized with k=10 folds. This method evenly splits the data set into 
training and testing folds, then iteratively training k-1 = 9 folds of data with a model, then testing against the remaining fold. This test is what generates a performance
metric calculation. With k=10 folds, we generate 10 different sets of performance metrics and finally are able to take the mean of these metrics for each model following the
final iteration of the algorithm. With this, we have been able to generate the performance metrics observed in the table below.

| Model | RMSE | R-squared |
|--------|--------|--------|
| GAM | 5.03628 | 0.32219| 
| NW | 3.71250 | 0.63122|

To better understand how our models performed, we can also plot the residuals for each model and see. 
### GAM
![residsnw](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project_4/GAM_residualsplot.png)
![resids nw](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project_4/resids_GAM.png)

### NW
![residsgam](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project_4/NWKernel_residualsplot.png)
![resids gam](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project_4/resids_NW.png)

From the above plot of residuals, we can easily see the normal distribution of the GAM's residuals yet there appears to be a slight deviation from a mean of 0 in the 
residuals. We can observe there to be a slight negative skew from the mean on residuals for the GAM plot. The NW kernel density estimation, also appears to have a normal 
distribution of residual errors, indicating that the NW Kernel Density Estimation is more effective in estimating the RMSD variable for this data set. Yet from these two 
histograms of residuals, we can certainly observe the better performance of the NW Kernel Density Estimation as the residuals more closely follow a normal distribution 
relative to the residual plots for the GAM.  The quantile plots can also show us how the data fits to a normal distribution. For the GAM plot, we see the normal distribution
represented by the red line plot at y=x, while for the NW plot we can observe the normal distribution centered around x=0. From this, we learn that a majority of the 
residuals for the NW plot are centered around x=0 while the GAM plot of residuals showcases more points deviating from the red line. This is notable in the range of values 
where x < -1.25. These residuals greater than the normal distribution line showcase a skew in the model and We can see for the NW kernel density estimation how the residuals 
certainly do not fit to the red line well, a strong visual indication of the relatively poor R-squared value that we observe for this method. Thus, we can say that for this 
application of models, the Nadaraya-Watson Kernel Density Estimation method performed most admirably of the two!
