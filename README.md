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
For sake of comparing different models, we use the Mean Absolute Error (MAE). This value is an arithmetic mean of all of the absolute errors for a given model. More generally, every time a model makes a prediction against the test set how close that prediction was to the actual value is recorded as the absolute error. So, for every prediction within the test set, an absolute error value is calculated. Finally, the MAE is calculated by taking the mean of all of these mean values. Regressors always try to minimize error. 

![MAE_formula](https://github.com/caiettia/Thesis-Project/blob/main/CodeCogsEqn%20(2).gif)

Thus, a lower MAE value indicates a regressor performing well, while a higher MAE value indicates a regressor being less accurate in predictions. This is all measured in relativity when comparing regressors! 

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

When plot along the axis, we see that the model follows a very generalized trend we can see in the data. 

![lin_mod_plot](https://github.com/caiettia/Thesis-Project/blob/main/lin_mod.png)
While this is correct in the sense that it identifies a positive correlation between the two axes, this is not a strong identification of correlation. 
This is because it does not account for any sense of localized weighting between various regions on the graph. 
This is can be seen granularly between x=4 rooms to x=5 rooms. In this subset, a majority
of the data points are above the regression line. However, when looking at x=5 rooms to x=6 rooms, the opposite is true. This is one indication that the linear model
is a weak learner. This means that the model is better than chance when it comes to making predictions, but it is not very sensitive to local subsets of the data. Also, being 
a weak learner entails that the model will not be very sensitive to new data when introduced. This makes
it a nice starting point for our regression, but certainly not the best regressor we will identify. 

To further understand the rate of error of our linear model, we run our KFold with k=10 and find a Mean Absolute Error of $4,424.53. 

### LOWESS
Local regression is a form of regression that takes into account local points throughout the plot to identify the a curve of regression. This is 
done by the process of identifying local weights, which are determined through the use of different kernels or mathematical functions which weight a regression based on local subsets of the overall data set. There are a host of different kernels that can be utilized for weighting, yet only three will be considered for this project: tricubic, quartic, and the Epanechnikov kernel functions. 

Following a k=10 KFold implementation for each kernel, we observe the following: 

| Kernel       | MAE       |
|--------------|-----------|
| Tricubic     | $4,081.07 |
| Quartic      | $4,076.19 |
| Epanechnikov | $4,085.36 |
| Uniform      | $4,110.08 |

We can also plot these kernels to visually understand how they may differ. 

![diff_kernels_plot](https://github.com/caiettia/Thesis-Project/blob/main/diffkernelslowess.png)

Visually, we can see the same sense of closeness between the three kernels that we do when comparing the MAE values. Tricubic and Quartic kernels both give us similar curves when plot, signfiying that roughly $4 difference in the MAE. The Epanechnikov kernel is notably more performant relative to the Tricubic and Quartic functions. We can see this visually in its slight divergence at points from the other two kernels, and also the $16 to $20 difference from the other two kernels. So, we identify the Epanechnikov kernel function as the most performant of the three and the choice as the best representative for LOWESS. 

### Support Vector Regression
For purposes of understanding what regressor would be best applicable for our dataset, we can explore the various kernels for SVR. More specifically,
we will look at the linear, polynomial, and radial basis function (rbf) kernels for SVR. When considering what parameters to pass to
each regressor, the case of the polynomial kernel requires one to observe the data to see what would fit best. Upon observation of the trend of the data, a polynomial of the 
second or third degree both appear potentially viable for this problem. I recognize this by seeing what appears to be a second or third degree trend in the data. For sake of 
learning, I will keep the fourth degree polynomial included, because this can validate our inference of the trend in the data based on how it fits to the data.
All of these kernels dictate how the decision boundary is identified for a data set in a given SVR. To better visualize the various kernels with our data set, the plot below is 
generated:


![svr_kernels_sep](https://github.com/caiettia/Thesis-Project/blob/main/svr_kernels_sep.png)

![svr_kernels_tog](https://github.com/caiettia/Thesis-Project/blob/main/svr_kernels_together.png)

We can see based on the plot how, when given a kernel, this affects the shape of the regressor. The linear kernel obviously creating a linear regressor, the polynomial kernel 
creating a polynomial regressor of what visually appears to be of the fourth degree, and the radial basis function producing a . 

We again apply KFold for k=10 and see the following values for our MAE:

| Kernel       | MAE       |
|--------------|-----------|
| linear       | $4,434.24 |
| rbf          | $4,124.47 |
| polynomial (deg = 4)   | $36,017.88 |
| polynomial (deg = 3)   | $4,242.55 |
| polynomial (deg = 2)   | $4,155.11 |

Based on the output MAEs from Kfold, we can see that indeed our inference about the polynomial kernel for SVR was correct! The fourth degree result is certainly nonsensical, 
while the second and third degree results are more agreeable. While the second degree polynomial performs best of the three polynomial kernels, the radial basis function 
kernel still outperforms them all. Thus, from the SVR camp, we choose the rbf kernel SVR for further comparisons.

### XGBoost
Extreme Gradient Boost, or more colloqiually known as XGBoost, is a form of Gradient Boost that is particularly effective at combatting overfitting. This is thanks to the 
ability for XGBoost to regularize parameters. Gradient Boost, on the other hand, is a form of regression that utilizes an ensemble of decision trees to make predictions. 
We can visually see XGBoost's performance on our data set below: 

![xgb_plot](https://github.com/caiettia/Thesis-Project/blob/main/xgboost_plot.png)

Following k=10 KFold, an MAE of $4,167.28 is observed.

### Sequential NN
A Sequential Neural Network is a form of constructing a neural network, where each layer of the NN is built sequentially. So, each iteration of the sequence builds a 
number of neurons within the network, until finally the network is built and fit with the training data. For the purposes of this proejct, the model is sequentially built 
starting with 128 neurons, then 32 neurons, then 8 neurons, then finally a singular neuron for the last layer. The first three layers have a rectified linear activation 
function, while the final layer has a linear activation function. The reason this final layer is composed of one neuron is because this is the output layer, and is necessary 
since we are utilizing this NN for a regression task! 

The NN is then compiled with the ADAM optimizer, and finally fit with the training data. We can see the model fit below:

![nn_plot](https://github.com/caiettia/Thesis-Project/blob/main/nn_plot.png)

Finally, we can get a good quantification of the model performance through a k=10 KFold. From this, we see an MAE of $3,895.27.

## Final Regressor Evaluation
To visually see how each regressor is fit to the data, we are able to plot the regressor ontop of eachother. 

![various_regressors](https://github.com/caiettia/Thesis-Project/blob/main/various_regressors.png)

What is particularly interesting about this plot, is just how well it visualizes the idea of a weak versus strong learner. Looking at the Linear model in red, we see how from 
x = 8 rooms to x = 9 rooms, the linear model does not necessarily account well for the newer observations. Yet looking at the NN and LOWESS regression, we see how each of 
their curves responds to the shifting weight of the data set. Looking again from 8 to 9 rooms, we see how these two regressors are able to adequately accomodate for the 
weight of data points being notably higher along the y axis, when compared to the general data set.

Another interesting observation can be seen between x = 6 rooms to x = 7 rooms. Here, the weight NN is not as easily able to fit to the data as the LOWESS regressor. The 
LOWESS regressor seems to be much more sensitive to local subsets of the data set when compared to the NN. This can also be seen between x = 4 rooms and x = 5 rooms. Again, 
the LOWESS regressor is able to learn better from the small subset of data when compared to the NN and linear model. So, we can see that the Epanechnikov Kernel Function 
makes the LOWESS regressor much more sensitive to new data, and thus a strong learner.

From the above explanations of each regressor, we have seen varying levels of performance, quantified by the MAE. From the table below, we see that the locally weighted 
regression (LOWESS) utilizing the Epanechnikov kernel has the smallest MAE relative to the other regressors. Thus, for the purposes of this data set, LOWESS is the best 
regressor! While neural networks are increasingly regarded as the most advanced or accurate methodologies, this data set is just another example of seemingly simpler 
regression techniques still performing on par or better than more advanced techniques that we may encounter.

| Model       | MAE       |
|--------------|-----------|
| Linear Regressor   | $4,424.53|
| XGBoost         | $4,167.28 |
| SVR (rbf)     | $4,124.57 |
| Sequential NN   | $3,895.27|
| Lowess(Epanechnikov)   | $3,779.39|

