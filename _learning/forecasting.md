---
title: "Forecasting"
excerpt: "See the future"
---

## What is forecasting?
Prediction of future events for planning purposes

All forecasts are wrong. They're more accurate the closer you get. They should always include uncertainty.

We want to look at patterns:
- Trends
- Seasonality
- Cyclical
- Autocorrelation
- Random Variation


## Methods

Simulations: distributions and randomness and see the results

Casual Relationships: Linear regression

Time Series: Predict future demand based on past history patterns




## Exponential Smoothing

Exponential smoothing is type of time series analysis that depends mostly on the most recent observation and the error of the latest forecast. Used mainly for random patterns, not seasonal patterns or cyclical.

States that demand is based on the last demand. It is easy to understand, doesn't require a ton of data, pretty accurate, fast to calculate.


$F_{t+1} = F_t + \alpha (A_t-F_t)$

where $t$ is the time, $A$ is the actual, $F$ is the forecasted value, $\alpha$ is a value between 0 and 1 that decides how important the error was. $\alpha$ as high, that means it really is dependent on the error. This leads to responsive, low $\alpha$ is stable.

You have to have an initial forecast. Can set it equal to the actual value. Can use another method.


### Adding Trending
Vanilla exponential smoothing just accounts for randomness. It doesn't really capture trends. It can't keep up with the trend; it lags.

$T_t = T_{t-1}+\delta(F_t-F_{t-1})$

$F_t = F_t + T_t$

### Adding Seasonality
This can include cyclicals.

Seasonality Index: $SI = \frac{Period Average Demand}{Average Demand All Periods}$

De-seasonalize demand and forecasts.

$A_t=\frac{A_t}{SI_t}$ and $F_t=\frac{F_t}{SI_t}$


### Errors

The error is just the difference between actual and forecast

$E = A_t - F_t$

#### RSFE - Running Sum of Forecast Error
$RSFE=\Sigma{(A_t-F_i)=\Sigma e_i}$

Just keep adding your error up as you go.

#### MFE - Mean Forecast Error (Bias)
Average error in the observation. Answers *is your average 0*?

$MFE=\frac{\Sigma (A_i-F_i)}{n}=\frac{RSFE}{n}$



### MAD - Mean Absolute Deviation
Are you consistantly 0?


$MAD = \frac{\Sigma |A_t-F_t|}{n}$
Same as MFE but take absolute value

## TS - Tracking Signal


$TS = \frac{RSFE}{MAD}$
