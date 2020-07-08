---
title: "Boosting"
excerpt: "Machine learning that sounds cool"
---

## What is Boosting?
Machine learning algorithm to create classification model

### No Algorithm is Perfect
No single algorithm is going to be most accurate. Every algorithm has its weaknesses and draw backs. What if we could combine all the results of several classifiers?

Have several basic classifiers (called *v classifiers* or *learners*) and combine them to form a *strong classifier*.

**Decision Stump:** A simple decision that uses only one feature. Basically a horizontal or vertical line in your data set (not diagonal, that would be multiple dimensions).

$$h(x;\theta_j)=sign(w_jx_j+b_j)$$

**Boosting is how to combine all these week learners.**



## Adaboosting
The *Ada* comes from *Adaptive*. The idea is you take a classification model (weak classifier) and use the training data to teach that classifier. Wherever the classifier performs well, that model is implemented. But wherever the classifier failed, that data is then sent to learner two to see if it can do better. This continues with classifiers seeing a subset of the data and attempting to classify well. Each classifier focuses on the samples that the previous classifier was not able to classify right.

You have to weight the samples and we use *D* to denote the weights. 
