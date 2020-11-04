---
title: "Regressions Trees in Random Forest"
excerpt: "Picture acres and acres of linear regression"
---

## What is Random Forest?

Random Forest is much more than a great sounding name for a band or some complex metaphysical philosophy.

But first we have to learn what a regression tree is first before we understand a forest.

## First Regression Tree

Regression trees partition the feature space into a set of rectangles. Each rectangles gets a linear function, combine them together? Regression tree.

It ends up looking like a piecewise surface plotted.

Occasionally your data will be nonlinear (gasp!).  Linear regression alone will fail you, but you can use Regression Trees to create a piece-wise regression.

How to solve? Take one variable and split it somewhere. Find the linear regression for each side of the split that best fits the data on that side.

Easy to overfit, have to make sure you prune to prevent overfitting. But have enough nodes to have good prediction. Your depth is important! You'll look at the cost function which considers the errors and the complexity.

This technique can also be used for classification!

### CART


## Back to the Forest

Okay, now that we understand trees, let's talk about forests. Trees have good computer efficiency but can have low predicting accuracy. They can also be noisy. To improve accuracy and robustness, we can try to combine trees. The idea being combining a bunch of algorithms to enhance performance.

How do we combine them? Just take the average!


## Ensmble Methods
Choosing a machine learning algorithm can be hard. If you can't decide, you can combine.

Bagging = Bootstrap Aggregating. This is when you make multiple models from the same data set using sampling.

You use a majority vote.

You don't really need cross-validation with bagging.
