kernel---
title: "Density Estimation"
excerpt: "Like histograms, only better"
---


Density = capturing *the shape* of the data

If you look at a space, what is the probability of observing data in that space

What's the likelihood of seeing a data point like this? Is it typical, or an outlier?

A simple way of showing data shape is histograms. Typically, 1D, marginal histograms are plotted to show where data lies.

If you want to capture two variable shapes at ones, one can make a contour plot on a scatter plot

Parametric Models: Fit a parameter to a distribution to best represent the density distribution. Fitting to Bernoulli or gaussian distribution are examples.

Nonparametric Models: No assumption of a distribution, but use shapes to describe the data. Histograms or Kernel density estimator are examples.

Density graphs are pretty much just histograms, but wherever there is a datapoint you draw one unit gaussian curve. You add the curves for each point on the graph. Hence you get pretty, smooth shapes. It's called kernel because because you choose some function to represent a point, in this case, a gaussian function.
