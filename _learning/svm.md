---
title: Support Vector Machine (SVM)
except: Get to know what SVM actually means
---


The intuitive thought process for SVM is you want your decision boundary to not be very close to any of your data points. That gives the classifier some margin room between the groups and allows for higher accuracy.

This translates in math terms into: $$(w^Tx+b)y\geq c$$ where *w* is the weights, *y* is the class, and *b* is a scalar offset. Also, *c* ends up being your margin on either side, hence the hull margin is: $\gamma = \frac{2c}{||w||}$

Find the decision boundary that maximizes the margins. This is almost like finding the line that least fits the data (or best seperates it).$$maximize\frac{2c}{||w||}$$ subject to $$y^i(w^Tx^i+b) \geq c$$

The magnitude of C is dependent on *w* and *b*. Hence, it is really a factor of *w* and we can set C to 1. This simplifies to maximizing a margin of $\frac{2}{||w||}$ which is really minimizing the *||w||* function.

How many constraints do we have? Well each one of the training samples is a constraint...which means it can be a lot of constraints. But really, not every point is all that useful...only those are are close to the decision boundary. The points far from the boundary really aren't that important. In the end, there aren't that many points that are that important.



This can be seen mathematically:

Lagrangian Function:
$$L(w,\alpha,\beta)=\frac{1}{2}w^T w + \sum_{i=1}^{m} \alpha_i (1-y^i(w^T x^i+b))$$

KKT conditions exist.

Take the derivative with respect to *w*

$$\frac{dL}{dw}=0=w- \sum_{i=1}^{m}  \alpha_i y^i x^i $$

Here you can solve direction for *w* by just moving it to the other side.

$$w = \sum_{i=1}^{m}  \alpha_i y^i x^i $$

Hence you can see *w*, the weights, are directly linked to the data values: the x's, the y's, and the duals. If you multiply each and sum that value, you can find the w.

Take the derivative with respect to $\beta$

$$\frac{dL}{d\beta}=0= \sum_{i=1}^{m} \alpha_i y^i$$

From the KKT conditions, we know $\alpha_i g_i(w) =0$. And then $\alpha_i \geq 0$ with $g_i(w) \leq 0$. This means either $\alpha_i$ or $g_i$ have to be 0. Any point which has $\alpha_i$ will be taken out of the sum and not contribute to the decision line. The remaining points are the support vectors.  
