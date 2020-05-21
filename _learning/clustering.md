---
title: "Clustering"
---

# GOAL:
Divide objects into groups
Objects within group are more similar than out of group


# Examples:
- Clustering images: dogs from cats, sunsets from trees
- Clustering flowers: Iris data set
- Hand written letters: MNIST data set


# Kmeans Clustering

### How to do K means in Matlab
https://www.mathworks.com/help/stats/kmeans.html

1) Define k number of groups
2) Randomly place centers in spaces
3) Assign each sample to a cluster by a distance measurement
4) Repeat until cluster centers do not change


Will different intitlization lead to different results?
Yes! Of course. This algorithm is suseptible to local optima.

Will the algorithm always stop after some iteration?
Yes.

In order to find the global minimum, you'd have to enumerate through all possible combinations.

Objective function is always decreaseing and hence will converge, but to local

Euclidean distance is typically used but doesn't have to be used. Can define similarity however you want.

Some examples of distances:
say we have: x = (x_1,x_2,x_3...) and y = (y_1,y_2,y_3...)

Euclidiean:
d = /sqrt(\Sigma (x_i-y_i)^2)

Minkowski:
d = (\Sigma(x_i-y_i)^p)^(1/p)
p = 1? Manhattan distance
  Also called hamming distance when all features are binary
p = 2? Eucliedean distance
p = inf? max of x_i - y_i

Edit Distance:
How much effort it is to edit something into something else.
Can vs Man = s, nothing, nothing
i = insertion | d = deletion | s = substitution
Can put penalties on each action



# Spectral Clustering
K-means is good at capturing "closeness" of data. However, it fails at capturing geometry. Hence, it cannot properly clustering two coincentric rings.
Spectral clustering focuses on partitioning graphs and considering connectivity.

You look at the network and form an Adjacency Matrix (A) and Degree Matrix (D).
A shows which nodes are connected together.
D shows how many edges each node has (on the diagnal).
If you take D-A, you end up with L, the Laplacian.
Note this Laplacian matrix will have rows and columns summing to zero.

Take the Eigen-value decomposition of the L matrix. You'll take the smallest Eigen-values's vectors and create a Z vector.

Then row K-means on the rows of the Z matrix.

Here's a short example/explanation: https://dinh-hung-tu.github.io/spectral-clustering/
TDS has a good write up as well: https://towardsdatascience.com/spectral-clustering-82d3cff3d3b7
