---
title: "Gaussian Mixture Models"
excerpt: "Using curves to classify"
---

#### Good example
[https://towardsdatascience.com/how-to-code-gaussian-mixture-models-from-scratch-in-python-9e7975df5252]

#### Good notebook
[http://ethen8181.github.io/machine-learning/clustering/GMM/GMM.html]
[https://colab.research.google.com/drive/1PChVghOtJSjWwCYVoevnLGXjnbQNvFOY#scrollTo=cQ2UWSEs_tpF]


GMMs assume all data points are mixture of Gaussian distributions, with unknown parameters.

It's actually quite similar to K-means, but unlike K-means, GMMs can learn clusters with any elliptical shape, not just circles. Also, GMMs give probabilities of belonging to a cluster, not hard assignment. This means a point can belong 50% to one cluster and 40% to another.

GMM can also be useful for outlier detection. Points with low likelihoood can be labeled as outliers.
