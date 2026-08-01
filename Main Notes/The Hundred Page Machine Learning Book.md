Tags:
- [[AI ML]]
---
## intro
- types of ML
    - supervised: labelled feature vectors => model that takes feature vector and predicts the label
    - unsupervised: feature vectors => model that takes feature vector and transforms it into another useful values (e.g. clustering, outlier detection)
    - semi-supervised: mix of labelled and unlabelled feature vectors => similar to supervised learning model
    - reinforcement learning: machine perceives state, executes actions, and receives rewards => learn a **policy** that takes in feature vector (the state) and finds the optimal action to take

## Notation and Definitions
- **parameters**: properties that define the model, set by the learning algorithm
- **hyperparameters**: properties of the learning algorithm, set by the developer
- **classification**: the problem of assigning the right label to an unlabelled example
    - e.g. `sick` vs `healthy`
- **regression**: predicting a real value given an unlabelled example
    - e.g. `the price will be $x`
- **model based** learning algorithms: use training data to create a model
- **instance-based** learning algorithms: the dataset is the model (e.g. kNN)

## fundamental algorithms
- linear regression
- logistic regression
- decision tree
- SVM (support vector machine)
    - extensions: **hinge loss** function to deal with noise, **kernel trick** to deal with non-linearity
- kNN (k nearest neighbours)

## anatomy of a learning algorithm
- components
    - loss function
    - optimization criterion based on that loss function (e.g. cost function)
    - optimization routine to fund solution to optimization criteria
- gradient descent: every epoch, for each parameter, decrement by the learning rate $\alpha$ * the partial derivative of the loss function w.r.t that parameter

## Basic Practice
- feature engineering techniques
    - one-hot encoding: turning categorical data into numerical
    - binning: turning numerical data into categorical
    - normalisation: transforming the values to fit within some standard range like `[-1, 1]` or `[0, 1]`
    - standardisation: transforming the values so the distribution follows a standard normal distribution
    - data imputation: replacing missing value
- data sets
    - training
    - validation
    - test
- regularisation
    - forcing learning algo to build a less complex model
    - increases bias (underfitting) but significantly reduces variance (overfitting)
- measuring model performance
    - confusion matrix
    - precision, recall, accuracy, AUC

## Neural Networks and Deep Learning
- Neural Network: nested function where each function contains a linear function and an activation function
- Deep Learning: training a NN with > 2 non-output layers
- Convolutional Neural Networks
- Recurrent Neural Networks

## Problems and Solutions
- Kernel regression: deal with non-linear relationships in the data
- classification 
    - multiclass: extend the usual binary models (e.g. logistic regression, decision trees) (e.g. using softmax)
    - one-class: only use examples of 1 class to build a model to distinguish it from everything else, requires special learning algos e.g. one-class kNN, one-class SVM, etc
- multi-label classification: assigning multiple labels to the same example: use binary cross-entropy as the cost function
- ensemble learning: train multiple _weak learners_ and take the result of all of them
- sequence labelling: `[input1, input2, ...] -> [label1, label2, ...]`
- sequence to sequence learning: `seq -> encoder -> embedding -> decoder -> seq`
- active learning: when labelling is expensive. Train the model, identify the most important examples (based on density and uncertainty), get those labelled, retrain and repeat
- semi-supervised learning: small fraction of the dataset is labelled
- one-shot learning: check if 2 images are the "same thing", uses siamese neural networks
- zero-shot learning: train model to label objects, even when they weren't in the training data

## Advanced Practice
- oversampling / undersampling: to adjust imbalanced datasets
- averaging / majority vote / stacking: to combine model results
- handling multiple inputs: vectorise or split into 2 models and combine later
- handling multiple outputs: a few layers to generate the embedding, then multiple subnetworks on top of that to get the outputs
- transfer learning: "transfer" some of the layers from a trained model to an untrained one

## Unsupervised Learning
- density estimation (estimating probability density function)
- clustering
- dimensionality reduction
- outlier detection

## Other Forms of Learning
- metric learning: let the model learn its own metrics too
- learning to rank
- learning to recommend
---
Source: https://www.goodreads.com/book/show/43190851-the-hundred-page-machine-learning-book
