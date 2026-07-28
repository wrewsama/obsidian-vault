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

---
Source: https://www.goodreads.com/book/show/43190851-the-hundred-page-machine-learning-book
