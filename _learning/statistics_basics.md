---
title: Statistics Basics
except: Stats Words That Get Thrown Aroudn
---




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
