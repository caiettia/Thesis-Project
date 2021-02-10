## 2/10/2021
lecture 6 / 7 .pynbs on google collab

Kernel locally weighted regression
 * a method that can be explained (not a black box like neural networks)
 * use KFold crossvalidation to validate LOWESS results

did random forest, linear regression, local weighted kernel regression
 * Mean absolute error quantified the predictive power of the model
 * linear regression is a weak learner in this case
 
Showed how to build an SVM regressor
 * choose from different kernels: linear, rbf, polynomial

implementing a neural net on the boston housing dataset
 * to start, import functions
 * why the number of hidden layers = 3?
   * there is no one theoretical compass to tell you how many 
   * should try different experimentations to identify what works best for your data
     * this is a point of criticism for NN
     * design is always relative to what you're proposing
 * activation function is relu, meaning rectified linear function
 * input dimension specifies what the dimensionality of the input data will look like
 * last layer has one and activation is linear because it is a regressor
 * the loss function is mean squared error because that was the one from paper
 * ADAM is the gradient descent optimizer and the parameters arent to be worried about right now
 * validation split
   * still need some validation with the neural network beyond the train and test split
   * that is what this validation split does
   * building of the model cannot access the real test data
   * reserve a chunk of data for neural network feedback
 * number of epochs
 * batch size = 100
 * callback refers to the early stopping (what kind of patience to wait for training of the network)

