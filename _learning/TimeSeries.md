---
title: "Time Series"
excerpt: "All About Time"
---

# What is Time Series?

Times series data is the same response that is known for many time periods.

Time series are some of the most common data types in modern life. Stock prices, historic weather, product prices, growth over time, blood pressure over time, daily sales, and a lot more are all measured over time.

Time series can have may factors, some that trends and some that are cyclical. You might have trends over macro periods of times, but within the macro, the micro might be different. Cycles can be on a month scale (temperature is colder in December than June, but every year, the temperature is going up). But there's also just randomness!

## What Questions Are Important With Time Series?

Let's take an example.

### Grocery Store Visits
Let's take me going to the grocery store, one of my least favorite chores.

I go to the grocery store...sometimes. Let's say I collect all the data of when I go to the store. What questions can I answer?

Raw Time:
1) When did I go to the store in raw terms? Like on a calendar.
2) What is my intermittent store traveling? Aka how many days pass before I go to the store?

Days of Week:
1) What days of the week did I tend to go to the store?
2) Is there a day of the week I didn't go to the store?
3) Is there a day of the week I usually went to the store?

Month:
1) What month was I going to the store the most? Maybe shows trends.

Day:
1) Did I ever go to the store within the same day?

Micro:
1) How long did I shop at the store?
  - what about trends? Does this change over time? By day of week?
  - What does that tell us?
      - sometimes I go just to pick up...does that show when I'm stressed or busy?

Streaks:
1) What's the longest I've gone without going to the store?
2) What's the shortest I've gone between trips?
3) How many consecutive weeks did I go to the store?
4) How many consecutive days did I got to the store? (Hopefully none, but it is a few)

Other actors:
1) What changes if I include my wife in this analysis?
2) How often do I go when she does? What about separately?
3) Do I spend more or less time when we go together?




Let's change the scenario just to see if I covered them all.

### When Does My Dog Sleep?

What if I want to look at when my dog sleeps?

Inner Day:
1) What hours of the day did she fall asleep today?
2) How long was she asleep today in total? Percent of day?
3) How does what hour of the day affect when she fell asleep?
4) What's the earliest she's woken up? What's the latest she's gone to bed?
4) When is her latest nap? Earliest nap?
5) How many naps does she take?
6) What is a nap even?

Day of Week:
1) Does she sleep less on the weekends because I'm home?
2) Does she sleep most on Sunday because I like to nap on Sundays?
3) What day of the week does she sleep the most? the least?

Raw:
1) Has she slept more since she was born? Or less?

Streaks:
1) Longest nap? Sleep? Shortest?
2) Day with most sleep? Least?


Other Factors:
1) Does she sleep longer when she's played more?
2) Do naps ruin her actual sleep?


## What is Forecasting?
Forecasting is the use of a model to predict values based on previously observed values.

All forecasts are wrong. They're more accurate the closer you get to the date (further out, the harder it is to predict.). They should always include uncertainty.

We want to look at patterns:
- Trends
- Seasonality
- Cyclical
- Autocorrelation
- Random Variation


## Methods

- Simulations: distributions and randomness and see the results

- Casual Relationships: Linear regression

- Time Series: Predict future demand based on past history patterns




## Exponential Smoothing

Exponential smoothing is a time series forecasting method for univariate data.

###### Why Use It?

It is easy to understand, doesn't require a ton of data, pretty accurate, and fast to calculate.

###### What is it?

The prediction is a weighted sum of past observations. Single Exponential Smoothing (SES) is used mainly for random patterns, not seasonal patterns or cyclical. We can add extra terms to deal with those.

$$S_t = \alpha \cdot y_{t-1}+(1-\alpha)\cdot S_{t-1}$$

Where $S_t$ is today's prediction of $y$. $y_{t-1}$ is yesterday's actual value, $S_{t-1}$ is yesterday's prediction, and $\alpha$ is a constant value that weights those values.

$\alpha$ is a value between 0 and 1 and can be chosen by minimizing the mean-squared error (MSE). An $\alpha$ close to 1 indicates *fast* learning as only the most recent point is valued.  An $\alpha$ close to 0 means the learning is *slow* as past observations have a heavy influence on the forecast.

###### Why Is It Called Exponential?
The equation really appears to not have an exponential terms, so why is it called exponential?

Well you have to get a few iterations in before you see it, but if you substitute in what $S_{t-1}$ actually is?

$$S_{t-1} = \alpha \cdot y_{t-2}+(1-\alpha)\cdot S_{t-2}$$

And what is $S_{t-2}$?

$$S_{t-2} = \alpha \cdot y_{t-3}+(1-\alpha)\cdot S_{t-3}$$

So let's plug those in:

$$S_t = \alpha \cdot y_{t-1}+(1-\alpha)\cdot (\alpha \cdot y_{t-2}+(1-\alpha)\cdot (\alpha \cdot y_{t-3}+(1-\alpha)\cdot S_{t-3}))$$

Do some rearranging and you'll start to see the exponentials.

$$S_t = \alpha \cdot y_{t-1} + \alpha\cdot(1-\alpha)\cdot y_{t-2} + (1-\alpha)^2 \cdot S_{t-2}$$




###### Alternative Way To Think About It:
Some people use Exponential Smoothing as:

$$F_{t} = F_{t-1} + \alpha (A_{t-1}-F_{t-1})$$

where $t$ is the time, $A$ is the actual, $F$ is the forecasted value, $\alpha$ is a value between 0 and 1 that decides how important the error was. $\alpha$ as high, that means it really is dependent on the error. This leads to responsive, low $\alpha$ is stable.

It relies depends mostly on the most recent observation and the error of the latest forecast.

###### Example:
You can think about a patient who gets their blood pressure taken daily. There is random variation on day to day basis that might cause issues. One measurement might be higher but does that actually matter? How can we remove noise from this data?


###### Notes:
You have to have an initial forecast. Can set it equal to the actual value, can use another method.

Can check out [NIST's page](https://www.itl.nist.gov/div898/handbook/pmc/section4/pmc431.htm). Simple but thorough.


### Double Exponential Smoothing (Adding Trends)
Vanilla exponential smoothing just accounts for randomness; it doesn't really capture trends. It can't keep up with the trend; it lags. We can fix this by adding a trending factor.




###### Alternative Form of Thinking

$$T_t = T_{t-1}+\delta(F_t-F_{t-1})$$

$$F_t = F_t + T_t$$

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


## What are some good tools?

### Data Viz
Being able to visualize trends and time series is key. Human brains can understand how things change over time and being able to visually check out what is happening is key.

### ARIMA

ARIMA is short for "Auto Regressive Integrated Moving Average". It attempts to use its own past values and the lagged errors to predict future values.

There are three key values: *p, d, q*

*p* is the order of the auto regressive term. It is the number of lags used to be predictors.

*q* is the order of the moving average term. It is the number of lagged forecast errors that should go into the error.

*d* is the number of differencing required. ARIMA models use linear regression and linear regression works best when the predictors are not correlated and are independent of each other. This is called making the time series stationary. Usually this is accomplished by taking a difference. Hence *d* is the minimum number of differencing needed to make the series stationary.

### SARIMA

### GARCH
