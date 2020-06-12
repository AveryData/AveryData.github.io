---
title: "Classification"
excerpt: "Does this data point belong in group A or B?"
---

## Classifying
This is a supervised learning method in which we have a matrix with labels to train our model.

It is similar to clustering, but clustering is unsupervised and hence contains no labels. Clustering wants to find inherent structures within a data set. Classification solves I have a new point and don't know where it belongs, use the past history to determine where it best fits.

## Things you can classify
- Anything that has groups
- Cats from dogs
- Different type of flowers
- Different types of customers
- Different flavors or scents
- Is this tumor malignant or benign?
- Who will this citizen vote for?


You need some "decision boundary" to split your data into the separate groups. It is usually said if you belong in certain spaces, you belong to that space's group. This space is easiest seen in 2D but can be in much deeper dimensions than two.


## K-nearest Neighbor
If you get a new point, calculate the *k* closest points, what groups do those points belong to? Weight them by the distance, majority rules.

- Non parametric
- Non linear
- Easy to kernelize

Relies on Vornoi partitions.

## Decision Tree

## Logistic Regression

Probability of seeing this class based on the data...set a threshold cut off to classify.

## Naive Bayes

Based upon multivariate gaussian distributions

**Bayes Rule**
$$ P(y|x)= \frac{P(x|y)P(y)}{P(x)} = \frac {P(x,y)}{\Sigma P(x,y)} $$
y is the label, x is the feature. What is the chance for the label to be one of these classes.

**Bayes Decision Rule**
Assign the data point to whatever group has the highest probability of belonging.

Can be difficult with lots of data, due to large covariance matrix. If you assume the covariance is the identity matrix, hence all the x's are independent this give you **Naive Bayes**



## Metrics
No model is perfect, but some are better than others. How do we know which to use?

### False Negative (Type II Error)
Falsely accept the null hypothesis
You actually have COVID19 and the test says you don't.

### False Positive (Type I Error)
Falsely reject the null hypothesis.
The test says you have COVID19 but you actually don't.

### Confusion Matrix
A confusion matrix shows the True Negatives, True Positives on the diagonol with, False Negatives and False positives on the opposite diagonal.

### Sensitivity
Sensitivity = True Positive Rate. Also called precision?
$ Sensitivity = \frac{true positive}{true positive + false negative}$

### Specificity
Specificity = True Negative Rate
$ Specificity = \frac {True Negative} {TrueNegative+FalsePositive}

#### Accuracy
How many your model predicted right
$ Accuracy = \frac{True Positive + True Negative}{All Data Points}

#### ROC
Receiver Operating Characteristic curve
Binary cut off classifier visualization.
Plot False Positive (x) by True Positive (y)
