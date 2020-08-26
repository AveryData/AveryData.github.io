---
title: "Error Metrics"
excerpt: "How Good is Your Prediction? Or maybe, How Bad is Your Prediction?"
---

## Root Mean Square Error (RMSE)
The Root Mean Square Error (RMSE) is the standard way to measure the error of a regression model.

$RSME=/sqrt{\frac(){Prediction-Actual)^2}{N}}

It can be thought to answer, "How far off should we expect our model to be on its next prediction?"

As RMSE decreases, the model performance will improve.

When is RMSE too big? Well that depends on the units and scenario. The error will have units of the term and even then, those units are dependent on the example. An error of 10 might be fine if the units are mililiters, but if the units are kilobarrels, we might have a bigger issue. 15 inches when trying to locate a bomb in a city probably doesn't matter, 15 inches when doing a laster engraving on a phone, is everything.

Often the error in absolute terms doesn't matter, it only matters in relative terms. Is the error smaller than it was previously? Does this model help us solve the problem?

- Makes an assumption that the error are unbiased and follow a normal distribution.
- Squared prevents cancelling of positive and negative errors
- Highly affected by outlier values.
- Punishes large errors

MSE is the same as RSME but RSME takes the square-root to bring the error back into the units of of the term.

Because MSE (note no R) uses a squared difference, it'll always be larger than MAE. Error grows quadratically with RMSE and means outliers will really affect our error.


## Mean Absolute Error
MAE is the simplest of metrics. It is just the average error, above and below the true value.



## R^2
How does our model do compared to a flat line average?

$R^2=1-\frac{MSE(model){MSE(baseline)}}$
