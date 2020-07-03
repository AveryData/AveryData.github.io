---
title: "Neural Nets"
excerpt: "The sexiest of all statistics"
---

The idea of neural nets comes from biological neurons in the human brain.

There are million of neurons in our brain. Each is a small decision maker that takes information in and makes an output. One neuron ever makes a decision, but a unique layered, network of them.  

Highly parallel, distributed. Many weighted interconnections.

There are several layers. Each layer is going to be one decision unit. It starts with a *input layer*, and it ends with a  *output layer* with *hidden layers* inbetween. Input variables are taken into the input layer. The neurons take that input and do a linear combination to create outputs of which are fed to the next layer until the output layer where they are combined to the final decision.

Nonlinear neurons can have activation functions like:
- hard limiters: simple threshold
- piecewise linear: exceeding limits changes
- sigmoid: between 0 and 1.
- hyperbolic tangent: between 1 and -1
