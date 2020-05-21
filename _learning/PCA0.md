---
title: "Principal Component Analysis"
excerpt: "Learning about PCA"
---

# Data Dimensionality
There is a lot of data in the world, and that data has lots of data. Somtimes, it is too much data. Too much for a human to digest surely, but even sometimes too much for a computer to handle in reasonable amounts of time. In these cases, we choose some sort of dimensionality reduction to allow for simpler, quicker calculations.

We do this by not using the original data points ,but combine, transforming, and selecting variables.

It is especially useful in plotting as we can really only see 3 dimensions at one time.

This is also important for aggregating weak signals in data.

# Principal Component Analysis and the Mighty Multivariate Decomposition
Principal Component Analysis is the primary technique for data compression. It relies heavily upon linear algebra and eigenvectors.


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
