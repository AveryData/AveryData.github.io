---
title: "Optimization"
excerpt: "Find the best. With math."
---


## Convex Sets
Basically if you have a shape, and you draw a line from one point in the shape, to another, the entire line should be within the shape. It also requires the set to be closed with complete boundaries.  

### Convex Shapes
Common convex shapes include rectangular shapes in space, balls or ellipsoids, and polyhedra.

### Convex Properties
If you take the intersection of multiple shapes, it remains a convex set. Linear combinations of convex sets, also remain convex. Finally, projections and concatenations of convex sets are also still convex sets.

### Convex Function
Convex Function: a function is convex the domain is convex and the linear combinations of two points is completely above the shape. Think like a bowl shape function. Concave functions would be the opposite, like a hill and the line is always below the function.

First Order Condition: At any point, draw a tangent point, then that line must be below the function.

Second Order Condition: Take derivative twice, the function must have an upward curvature at each point. It is also called the Hessian matrix.

Exponentials, powers, norms, max functions, log determinates are all examples of convex shapes.




## Solving Logistic Regression
In logistic regression, we want to find parameters that allow for the best classification. To find these parameters, we actually use optimization.
