---
title: "Clustering"
excerpt: "Finding natural groups within the data"
---

## What is Clustering?
To group samples together into groups based on their similarity.

It is the most common of unsupervised learning; the data is not labeled.

Usually these samples do not have definite labels and are instead, unlabeled and we want similar samples to group together and hopefully a human can give those groups labels.


## Examples:
- Clustering images: dogs from cats, sunsets from trees (when the pictures are not labeled)
- Group patients together
- Figure out topics in papers
- Clustering flowers: Iris data set
- Hand written letters: MNIST data set
- Topic analysis in Tweets
- Segmenting Customers for sales or marketing
- Understanding relationships between complex systems


## Kmeans Clustering

1) Define k number of groups
2) Randomly place centers in spaces
3) Assign each sample to a cluster by a distance measurement
4) Repeat until cluster centers do not change


Will different initialization lead to different results?
Yes! Of course. This algorithm is susceptible to local optima. You should run a few times to make sure you know what's going on.

Will the algorithm always stop after some iteration?
Yes.

It is a bit time complex to solve.

In order to find the global minimum, you'd have to enumerate through all possible combinations.

Objective function is always decreaseing and hence will converge, but to local

Euclidean distance is typically used but doesn't have to be used. Can define similarity however you want.

### Some examples of distances:
say we have: x = (x_1,x_2,x_3...) and y = (y_1,y_2,y_3...)

#### Euclidiean:
d = /sqrt(\Sigma (x_i-y_i)^2)

#### Minkowski:
d = (\Sigma(x_i-y_i)^p)^(1/p)
p = 1? Manhattan distance
  Also called hamming distance when all features are binary
p = 2? Eucliedean distance
p = inf? max of x_i - y_i

#### Edit Distance:
How much effort it is to edit something into something else.
Can vs Man = s, nothing, nothing
i = insertion | d = deletion | s = substitution
Can put penalties on each action



### How to do K means in Matlab
[Matlab Demo](https://www.mathworks.com/help/stats/kmeans.html)


## Spectral Clustering
K-means is good at capturing "closeness" of data. However, it fails at capturing geometry. Hence, it cannot properly clustering two coincentric rings.
Spectral clustering focuses on partitioning graphs and considering connectivity.

You look at the network and form an Adjacency Matrix (A) and Degree Matrix (D).
A shows which nodes are connected together.
D shows how many edges each node has (on the diagnal).
If you take D-A, you end up with L, the Laplacian.
Note this Laplacian matrix will have rows and columns summing to zero.

Take the Eigen-value decomposition of the L matrix. You'll take the smallest Eigen-values's vectors and create a Z vector.

Then row K-means on the rows of the Z matrix.

[Here's a short example/explanation:](https://dinh-hung-tu.github.io/spectral-clustering/)
[TowardsDataScience(TDS) has a good write up as well:] (https://towardsdatascience.com/spectral-clustering-82d3cff3d3b7)

## Hierarchical Clustering
Build a tree or hierarchy of clusters.

A **dendrogram** is the tree shown that shows the clusters. There is also radial clustering dendrograms.

Also see cool visualizations with circles within circles.

Not super scalable, not good for millions of items or more. But really easy to understand and explore.


### How To Combine Clusters
**Single linkage:** merge 2 clusters whose closest members have the smallest distance.

**Complete linkage:** merge 2 clusters such that the result cluster has the smallest diameter (most dissimilar members)

**Average linkage:** distance between cluster centers. Very computationally efficient.

## DBSCAN
Density based spatial clustering with noise.

Only needs two parameters
- radius
- minimum number of points to form a dense region


Check out this [demo](https://www.naftaliharris.com/blog/visualizing-dbscan-clustering/)


## Visuals
- Bubble Chart
- Sun Burts
- Topic Matrix
- Dendogram

When visualization, the ordering is really important.

Use colors to show the clusterings.
