captures---
layout: posts
title: "Classification"
excerpt: "Does this data point belong in group A or B?"
tags: [data science, cool stuff, frozen]
---

## Classifying
What class or group does a sample belong to? If we had to label this sample, what would the label be?

This is a supervised learning method in which we have a matrix with labels to train our model.

It is similar to clustering, but clustering is unsupervised and hence contains no labels. Clustering wants to find inherent structures within a data set. Classification solves I have a new point and don't know where it belongs, use the past history to determine where it best fits.

## Things you can classify
- Spam or not spam?
- Sentiment Analysis (is this tweet positive, negative, or neutral)
- Is this face a baby, middle-aged, or old
- Anything that has groups
- Cats from dogs
- Different type of flowers
- Is this news article politics, sports, pop culture?
- Different types of customers
- Different flavors or scents
- Is this tumor malignant or benign?
- Who will this citizen vote for?

Based on previous songs, will this person like the new song?

### Recipe
You need data: attributes (x) and target (y). Some sort of model to map the data into a label within the target, y. Finally you need a loss function that minimizes mistakes.

### Main Approaches To Classify

- Bayes Rule $p(x|y=1)$
- Use geometric intutions
    - knn
    - svm
- Direct Decision Boundary
    - logistic regression
    - neural network


You need some "decision boundary" to split your data into the separate groups. It is usually said if you belong in certain spaces, you belong to that space's group. This space is easiest seen in 2D but can be in much deeper dimensions than two.


# Different algorithms

Here's a great demo of how different classifiers do on different data. [SKLearn's Classification Comparison](https://scikit-learn.org/stable/auto_examples/classification/plot_classifier_comparison.html)

## K-nearest Neighbor
If you get a new point, calculate the *k* closest points, what groups do those points belong to? You can weight them by the distance, and majority rules.

- Very intuitive
- Non parametric
- Non linear
- Easy to kernelize
- Few parameters
- Effective
- Computationally expensive

You can use different distance equations. Manhattan, Euclidean are too common ones.

Specifying how many neighbors to use is a key parameter. You can use cross-validation to understand what *k* might be good. If you're using less than 2 neighbors, you may be overfitting.

Relies on Vornoi partitions.

## Decision Tree
Think about twenty questions with Alexa. Is it an animal? No, leading to a new question. Is it this? Does it have horns? And so on. Find the optimal way to split based on misclassification errors. Similar to AdaBoosting, but not combining weak learners. Good computing efficiency but accuracy in predictions might be low. Hence we combine trees and do forests.

We start with the best split at that point. That creates a new node, and that node, you ask the same question, what's the best split for one family down.

You have to find the best split on the chosen attribute, what's the next best attribute to split on, when do you stop splitting, and then of course doing cross validation.

You'll need a loss function for choosing the split point. Choosing the attribute is the one that gives the highest information gain or that is the minimum in training loss. When to stop? Can't overfit. Could do it when nodes are pure, or purer than a threshold. Or you could do when the points get to a point.

Can work with any type of variables and in combination, works pretty well, easier to understand and interpret. Could be hard to cross validated and training is a bit expensive.


### Details

#### Entropy
Entropy captures the degree of purity of a distribution. An array with very similar frequency of terms has high entropy. If one category appears much more often than others, that array has low entropy.

$$H=-\Sigma{-P_i\cdot log_2(P_i)}$$

#### Information Gain
We want as pure as nodes as possible and reduce the entropy as much as possible.

Maximize the following:

$$IG=H-(H_l\cdot P_L + H_R\cdot P_R)$$


#### Best split 





## SVM
Kernel constructed decision bounderies based on minimizing the margin between the closest points. Easiet to think as linear, but can be non-linear including gaussian.

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

### Overfitting
The model must generalize to data we have not seen. It can't only work good on data we know the outcomes, it has to work on unknown data. The data where outcomes were known, this data is referred to as training data, in juxtaposition to the unknown set known as the testing set.

It's very easy to have 100% accuracy on your training data. You only need very millions of specific rules. But these rules will probably not generalize to other data. When this occurs, you are overfitting.

### Cross Validation
Cross validation is one technique we can use to prevent overfitting.

N-Fold Validation is when you divide the data into N-folds, N-1 of which are the training data, and the Nth is the testing data set. You repeat this N-1 times and compute your metrics and then take the average of the metrics.

Leave one out Cross Validation (LOO-CV) is when you have a test sample of 1; computationally draining.  

### False Negative (Type II Error)
Falsely accept the null hypothesis
You actually have COVID19 and the test says you don't.

### False Positive (Type I Error)
Falsely reject the null hypothesis.
The test says you have COVID19 but you actually don't.

### Confusion Matrix
A confusion matrix shows the True Negatives, True Positives on the diagonal with, False Negatives and False positives on the opposite diagonal.

It's just a table with one axis as actual class and the other class as the predicted class. If it were to be a perfect classifier, everything but the diagnol would be 0's.

Just having numbers with lots of values is hard, but if you use colors, it can help you identify hot spots.

Be careful with the word positive. Make sure it is clear what you were aiming for.

### Sensitivity
Sensitivity = True Positive Rate. Also called precision?

$Sensitivity = \frac{true positive}{true positive + false negative}$

### Specificity
Specificity = True Negative Rate

$Specificity = \frac {True Negative} {TrueNegative+FalsePositive}$

## Recall

#### Accuracy
How many your model predicted right

$Accuracy = \frac{True Positive + True Negative}{All Data Points}$

#### ROC
ROC = Receiver Operating Characteristic curve. Binary cut off classifier visualization. Plot False Positive (x) by True Positive (y). Top left corner is desirable; low false alarms, perfect positives. It is a series of numbers and trade off between false positives and true positive rates.

If you want one number to describe the action, use AUC (area under the curve).

#### AUC
AUC = area under the ROC curve. Close to 1 usually means good, but be careful; they can be misleading.



## Model Parameters vs Hyper-Parameters

#### Hyper-Parameters
You need to try and then figure it out
- *k* in k-NN
- # of Nodes in decision tress

Can be determined via CV and optimization strategies, i.e. grid search (fancy way of saying try everything).

#### Parameters
Can be learned or computed directly from the data
- Which attribute to split in decision tree
- Split point for an attribute







## Code



### Logistic Regression

##### R
#### Logistic Regression
'''{r}
logreg = glm(Survived ~ Sex, data = titantic, family = "binomial")
'''
