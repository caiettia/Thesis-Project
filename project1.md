# Comparing Regressors
When understanding what regression technique is best applicable for a situation, one needs to compare regressors. This means that one methodically looks at various approaches
for regression and identifies what approach best fits their data set. Given the nature of data and the vast number of approaches available for a problem, there is no true
one model that will always be best. Thus, we must compare regressors to best understand what models work best for our given data set. 

For this project, we are given the Boston Housing Dataset, a dataset surrounding the prices of homes in Boston and various datum surrounding them. While there are a multitude of different columns in the data set, we are going to be looking specifically at the price of houses relative to the number of rooms they have.

To start, we can see this data in a plot below and get a better visual understanding of what might be going on.

![blank_plot](https://github.com/caiettia/Thesis-Project/blob/main/blank_plot.png)

Visually, there appears to be some sort of positive trend within the data. This is a good start, as it will give us an idea of what to expect in terms of plotting various
regressors on these axes. But before we begin plotting, we must understand how we are going to evaluate our various regressors.

## Evaluation Techniques
### Mean Absolute Error
For sake of comparing different models, we use the Mean Absolute Error (MAE). This value is an arithmetic mean of all of the absolute errors for a given model. More generally, every time a model makes a prediction against the test set how close that prediction was to the actual value is recorded as the absolute error. So, for every prediction within the test set, an absolute error value is calculated. Finally, the MAE is calculated by taking the mean of all of these mean values. Regressors always try to minimize error. Thus, a lower MAE value indicates a regressor performing well, while a higher MAE value indicates a regressor being less accurate in predictions. This is all measured in relativity when comparing regressors! 

### KFold
KFold is a form of cross validation, where we iteratively test a model to better understand its predictive power through metrics such as Mean Absolute Error (MAE). KFold works by splitting up a training dataset into "k" folds. Then, the model is trained on k-1 "folds" of the data set and validated on the the final fold within a given iteration. KFold iterations k times, so finally the performance metrics are identified by taking the mean of the metrics from each iteration. It is particularly worth noting that each fold is equal in length, and a new fold is picked in each iteration of KFold as the validation set. 

An example of this algorithm can be seen in the image below, where an instance of KFold is run for k=4. 
![kfold](https://github.com/caiettia/Thesis-Project/blob/main/kfold_example.png)

KFold is a critical step in the process of evaluating various regressors, because it allows us to obtain less-biased estimations of error for a given regressor. This is thanks to how KFold "shuffles" the data into unique folds and then iteratively trains and tests against different combinations of these folds. This allows the estimated error to account for particularly "good" or "bad" train and test splits within the data that may come about from testing.

Without KFold, we could be getting biased evaluation metrics and thus choose a potentially overfit or underfit model for our regression problem. Thus, it is very important that we do not forget to utilize KFold in evaluating our variety of regressors. 

## Models 

### Linear Model
A linear model is the representation of a relationship between two variables, x and y, where any rate of change is identified as constant. This usually takes the form of 
![linear_model_eq](https://github.com/caiettia/Thesis-Project/blob/main/CodeCogsEqn%20(1).gif)

When plot along the axis, we see that the model follows a very generalized trend we can see in the data. While 
this is correct in the sense that it identifies a positive correlation between the two axes, this is not a strong identification of correlation. This is because it does not account for any sense of localized weighting between various regions on the graph.

![lin_mod_plot](https://github.com/caiettia/Thesis-Project/blob/main/lin_mod.png)

To further understand the rate of error of our linear model, we run our KFold with k=10 and find a Mean Absolute Error of $4,424.53. 

### LOWESS
Local regression is a form of regression that takes into account local points throughout the plot to identify the a curve of regression. This is 
done by the process of identifying local weights, which are determined through the use of different kernels or mathematical functions which weight a regression based on local subsets of the overall data set. There are a host of different kernels that can be utilized for weighting, yet only three will be considered for this project: tricubic, quartic, and the Epanechnikov kernel functions. 

Following a k=10 KFold implementation for each kernel, we observe the following: 

| Kernel       | MAE       |
|--------------|-----------|
| Tricubic     | $3,795.95 |
| Quartic      | $3,799.25 |
| Epanechnikov | $3,779.39 |

We can also plot these kernels to visually understand how they may differ. 

![diff_kernels_plot](https://github.com/caiettia/Thesis-Project/blob/main/diffkernelslowess.png)

Visually, we can see the same sense of closeness between the three kernels that we do when comparing the MAE values. Tricubic and Quartic kernels both give us similar curves when plot, signfiying that roughly $4 difference in the MAE. The Epanechnikov kernel is notably more performant relative to the Tricubic and Quartic functions. We can see this visually in its slight divergence at points from the other two kernels, and also the $16 to $20 difference from the other two kernels. So, we identify the Epanechnikov kernel function as the most performant of the three and the choice as the best representative for LOWESS. 

### Support Vector Regression
For purposes of understanding what regressor would be best applicable for our dataset, we can explore the various kernels for SVR. More specifically,
we will look at the linear, polynomial, and radial basis function (rbf) kernels for SVR. These kernels dictate how the decision boundary is identified for a data set in a given SVR. We again apply KFold for k=10 and see the following values for our MAE:

| Kernel       | MAE       |
|--------------|-----------|
| linear       | $4,434.24 |
| rbf          | $4,124,47 |
| polynomial   | $36,017.88|


### XGBoost
Extreme Gradient Boost, or more colloqiually known as XGBoost, is a form of Gradient Boost that is particularly effective at combatting overfitting. This is thanks to the ability for XGBoost to regularize parameters. Gradient Boost, on the other hand, is a form of regression that utilizes an ensemble of decision trees to make predictions. Following k=10 KFold, an MAE of $4,167.28 is observed.

### Sequential NN
Following k=10 KFold, an MAE of $3,915.13 is observed.

## Final Regressor Evaluation
From the above explanations of each regressor, we have seen varying levels of performance, quantified by the MAE. 
