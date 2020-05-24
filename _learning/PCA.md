---
title: "Principal Component Analysis"
excerpt: "Learning about PCA"
---

# Data Dimensionality
There is a lot of data in the world, and that data has lots of data. Sometimes, it is too much data. Too much for a human to digest surely, but even sometimes too much for a computer to handle in reasonable amounts of time. In these cases, we choose some sort of dimensionality reduction to allow for simpler, quicker calculations.

We do this by not using the original data points ,but combine, transforming, and selecting variables.

It is especially useful in plotting as we can really only see 3 dimensions at one time.

This is also important for aggregating weak signals in data.

# Principal Component Analysis and the Mighty Multivariate Decomposition
Principal Component Analysis is the primary technique for data compression. It relies heavily upon linear algebra and eigenvectors. It works well when variables are linearly correlated.


Goals:
1) Make the data set smaller
2) Finding or revealing hidden

Dimensionality Reduction: Reduces N dimensions down.

Captures the most variation in the data, with fewest dimensions. Variations are "signals" or information in the data.
Discovers variables or dimensions that are highly correlated or redundant, leading to simpler presentation.


Examples when to use it:
- Pictures. A simple picture could have millions of pixels, but you can recreate the image with a compressed version that is pretty much the same with only 10% the data load.
- Iris Data Set: classifying flowers based on their petals and sepal characteristics.

Good Resources:
- https://setosa.io/ev/principal-component-analysis/




# Steps
1) Estimate Mean and Covariance
2) Take the eigenvectors corresponding to the biggest eigenvalues
3) Compute reduced representation (project your data)




Truncated Singular Value Decomposition (SVD)
- Computes eigenvectors

Diagnal entries are single values





# Non-Linear Dimensionality Reduction

PCA is really only suitable when variables are linearly correlated.

Examples where PCA might fail:
- Two coincentric rings
- Swiss Roll example

How do you reduce non-linear dimensions?

First you need a way to measure distance. Since these day sets are based on connectivity, we can define distance as the sum of euclidean distances to get to one point to another, traveling through the data cloud.

This eventually leads to the idea of the Isomap

# Isomap

Step 1: Build a weighted graph A using nearest neighbors, and compute pairwise shortest distance matrix D.
Step 2: Use a centering matrix $H=I-\frac{1}{M} \cdot1 1^T$ to get a C matrix: $C=-\frac{1}{2m} H D^2 H
Step 3: Compute leading eigenvectors of the C matrix

PCA performs singular value decomposition on the data matrix and eigendecomposition on the covariance matrix.

Check out some isomap results:
https://www.researchgate.net/figure/The-Isomap-algorithm-when-applied-to-a-synthetic-face-database-of-698-images-The-number_fig5_250747602
https://benalexkeen.com/isomap-for-dimensionality-reduction-in-python/

Some other good resources:
http://blog.shriphani.com/2014/11/12/the-isomap-algorithm/
https://towardsdatascience.com/step-by-step-signal-processing-with-machine-learning-manifold-learning-8e1bb192461c
