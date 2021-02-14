# Comparing Regressors
We are given the Boston Housing Dataset, a dataset surrounding the prices of homes in Boston and various datum surrounding them. While there are a multitude of 
different columns in the data set, we are going to be looking specifically at the price of houses relative to the number of rooms they have. 
We can see this data in a plot below.
![blank_plot](https://github.com/caiettia/Thesis-Project/blob/main/blank_plot.png)

## KFold
KFold is a form of cross validation, where we iteratively test a model to better understand its predictive power through metrics such as Mean Absolute Error (MAE). KFold works by splitting up a training dataset into "k" folds. Then, the model is trained on k-1 "folds" of the data set and validated on the the final fold within a given iteration. KFold iterations k times, so finally the performance metrics are identified by taking the mean of the metrics from each iteration. It is particularly worth noting that each fold is equal in length, and a new fold is picked in each iteration of KFold as the validation set. 

An example of this algorithm can be seen in the image below, where an instance of KFold is run for k=4. 
![kfold](https://github.com/caiettia/Thesis-Project/blob/main/kfold_example.png)

KFold is a critical step in the process of evaluating various regressors, because it allows us to obtain less-biased estimations of error for a given regressor. This is thanks to how KFold "shuffles" the data into unique folds and then iteratively trains and tests against different combinations of these folds. This allows the estimated error to account for particularly "good" or "bad" train and test splits within the data that may come about from testing.

## Models 

### Linear Model
A linear model is the representation of a relationship between two variables, x and y, where any rate of change is identified as constant. This usually takes the form of 
![linear_model_eq](https://github.com/caiettia/Thesis-Project/blob/main/CodeCogsEqn%20(1).gif)

When plot along the axis, we see that the model follows a very generalized trend we can see in the data. While 
this is correct in the sense that it identifies a positive correlation between the two axes, this is not a strong identification of correlation. This is because it does not account for any sense of localized weighting between various regions on the graph.

![lin_mod_plot](https://github.com/caiettia/Thesis-Project/blob/main/lin_mod.png)

To further understand the rate of error of our linear model, we run our KFold with k=10 and find a Mean Absolute Error of $4,424.53. 

### LOWESS
Following k=10 KFold, an MAE of $3,799.25 is Quartic observed.
Following k=10 KFold, an MAE of $3,779.39 is Epanechnikov observed.
Following k=10 KFold, an MAE of $3,795.95 is Tricubic observed.

### Support Vector Regression
For purposes of understanding what regressor would be best applicable for our dataset, we can explore the various kernels for SVR. More specifically,
we will look at the linear, polynomial, and radial basis function (rbf) kernels for SVR. Following KFold with k=10 folds, we find that the MAEs yielded are as follows: 
linear $4,434.24, polynomial $36,017.88, and rbf $4,124,47.

### XGBoost
Following k=10 KFold, an MAE of $4,167.28 is observed.

### Sequential NN
Following k=10 KFold, an MAE of $3,915.13 is observed.

