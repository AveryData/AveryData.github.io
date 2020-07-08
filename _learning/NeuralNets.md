---
title: "Neural Nets"
excerpt: "The sexiest of all statistics"
---

## Background

The idea of neural nets comes from biological neurons in the human brain.

There are million of neurons in our brain. Each is a small decision maker that takes information in and makes an output. One neuron ever makes a decision, but a unique layered, network of them.  

Highly parallel, distributed. Many weighted interconnections.

There are several layers. Each layer is going to be one decision unit. It starts with a *input layer*, and it ends with a  *output layer* with *hidden layers* inbetween. Input variables are taken into the input layer. The neurons take that input and do a linear combination to create outputs of which are fed to the next layer until the output layer where they are combined to the final decision.

Nonlinear neurons can have activation functions like:
- hard limiters: simple threshold
- piecewise linear: exceeding limits changes
- sigmoid: between 0 and 1.
- hyperbolic tangent: between 1 and -1


If your network only has one layer (i.e. no hidden layers, just input and output) and you use a sigmoid kernel, it ends up equivalent to logistic regression.

The features are the same (qualities of the data) and the output is the classification prediction. Each feature has weights in the NN, which resemble the coefficients of logistic regression. They are staged in a linear combination of the features x their weights, which are then input into the logistic function.

ANN's can represent very complicated decision boundaries. That's why they perform well with big data in complex situations.

### Training Hidden Units

Training the hidden units is very similar to fitting logistic regression. But instead of a sigmoid function, you have a general activation function.

$$\underset{w,\alpha,\beta}{\text{min }} l(w,\alpha, \beta) = \sum_i^n(y^i-\sigma(W^Tz^i))$$

$z^i$ is the output of each hidden unit.

These are non-convex optimization problems without a closed form expression. Hence we use gradient descent to find the solution as a local optimum.


#### ImageNet
Large image dataset with 1.3 colored images with 1000 different classes. ANN's have been really successful in classifying the images.

**Convolution networks**: takes a 224x224 image and smashes it into 11x11 by finding ways of combining these pixels. Find the significant output from the previous layer and feed into the next layer. Combined in the end to make once decision.

It can be also thought of comparing the image to a template image.

Tons of parameters which means a lot of tuning and requires a lot of GPU. The more samples you have, the better your CN can be.
