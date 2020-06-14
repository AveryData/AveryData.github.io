---
title: Statistics Basics
except: Stats Words That Get Thrown Around
---


## Correlation vs Causation

### Correlation
Correlation is a measure of *linear relationship between X and Y*.

$ Corr(X,Y) = \frac{ um_{i=1}{n} ( (x_i - \bar x) (y_i - \bar y)     )  }   {\sqrt(\Sum_{i=1}{n}(x_i-\bar x)^2) \cdot \sqrt(\Sum_{i=1}{n}(y_i-\bar y)^2)  } $

A function like $$ y = x^2 $$ has perfect relation, but a correlation of 0, because non linear.

### Causation
If X and Y are heavily correlated. X could cause Y. Y could cause X. Y could case C which causes X. Or there could be another factor B that influences both.

**Correlation does not mean causation!!!!**

To get causation, you have to take out all other possible effects.


## Selection Bias
Individuals who are selected for treatment aren't actually random. Could be because they volunteered, the didn't volunteer, it was advertised to only one group.

You need to ensure you have a **randomized controlled experiment** that includes treatment and control groups.

Can have **natrual experiments** where the subjects don't have a choice really. In those experiments, you can see how the response variable changes and calculated *difference-in-difference* (DID). You basically have a treatment and control group, and a before treatment and after treatment time. It it defined as:
$ DID = \Delta Treated - \Delta Control = (AfterTreated - BeforeTreated) - (AfterControl - BeforeControl) $

This can be used in regression by creating a dummy variable of: Before, After, Control, Treatment. And an interaction term of control, after. 



## What is Log-Likelihood?
It is the log of the likelihood...what else do you need to know

Let's start with just likelihood

Usually we have a population that follows a pattern. We usually only have a sample...so our parameters in the sample might not match the population entirely. Hence, we use the best estimate that comes from maximizing the likelihood.

$$ Likelihood =  \prod_{i=1}^{N} f(x_i|\theta)$$
*the product of the individual density functions*

Choose $\theta$ to maximize this function. Well, think about you you find the max of a function? Find the derivative and set it to 0!

$$ \frac{\partial L}{\partial \theta} = 0 $$

You're finding the parameter, $\theta$ that maximizes the log-likelihood. Maximizing the log-likelihood is the same as maximizing the likelihood so why take the log?

Well taking the derivative of products is difficult, you'd need to use the product rule quite a bit. Hence, taking the log makes it easier because in log, you can change products to sum which makes the derivative much easier.
