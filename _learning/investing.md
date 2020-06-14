---
title: "Investing"
excerpt: "$$$"
---



## Returns
Return = Percentage change in stock price from one period to the next $$Price_2 - Price_1)/Price_1$$.

But you have to worry about stock splits and dividends. You end up with this: $r_t = \frac {p_tf_t+d_t} {p{t-1}} -1$
With *d* as the dividend and *f* is an adjustment factor for stock splits.

### Compound Returns
Total return over an entire period. The cumulative effect a series of gains and losses has on our initial investment.
$ Compound_Return = \prod_{i=1}^{n} (r_i+1) - 1$ It's basically the experience of an investor over time.

### Risk
You can rarely just evaluate one return alone, you must compare to others and understand how risky they are. Returns are just part of the story.

Treasury Bonds are very safe...but some are high risk, high reward. How to do quantify these?

#### Standard Deviation
Standard Deviation is *total risk* including firm specific and market wide component.
$ S = \sqrt( \frac {\Sigma(x_i-\bar x)}{n-1} ) $

Firm could be good or bad about the business specifically. But there are interest, global economy, pandemics ect.

If you hold a lot of stocks, the firm specific risk dissipates. We can separate the risk into a firm specific risk through linear regression.
$ r_i = /alpha + \Beta \cdot r_m + \epsilon $
With $$r_m$$ as the return of a broad stock index portfolio (think SP 500) and $$\beta$$ is a measure of a stock's sensitivity to overall market movements.

Then we use the $$R^2$$ to see what percentage of the fund's performance occurs due to the stock market. The higher the $$R^2$$ means the fund is more closely correlated with the stock market.

You can interpret $$\Beta$$ as a measure of risk. High $$\Beta$$ means high risk. The $$\Beta$$ value will range from 0 (no risk) to 1 (risk of market) to $$\infty$$.


#### Drawdown
Drawdown (DD) is the cumulative loss since losses started. It measures peak-to-trough decline in your investment. It can be defined as:
$ DD_t = \frac {HWM_t - P_t}  { HWM_t } $

Where $$HWM$$ is high-water mark and is the higest price a fund has achieved in the past.


#### $\Beta$ Values
If you fit the returns of the company as a function of the returns of the market, you'll be able to evaluate how risky that stock is.

A **coefficient on the market input** explains the risk factors. A coefficient of 1 means as risky as the market. Greater than one suggests riskier than the market, while less than 1 risks safer than the market.

The $$R^2$$ also shows how much of the profits are influenced by the market. High $$R^2$$ means high corrleation with the market.

#### Risk Adjusted Performance
Are our investments outperforming expectations?
$$ Abnormal Return = Actual Return - Expected Return $$

The easiest way can be to just to compare to an index.

#### Sharpe Ratio
The *Sharpe Ratio* is a measure of investment reward per unit risk; measure of award vs risk.

$$ Sharpe Ratio = \frac {R-R^f  } {\sigma (R-R^F)  

where $R$ is the return against $R^F$ is the risk free (think bond). High high Sharpe Ratios indicate better performance.

In R, using the library(PerformanceAnalysis), this can be calculated easily:
```{r}
SharpeRatio(berkshire_xts$BrkRet)
```

#### Treynor Ratio
Similar to Sharpe except the denominator is not $\Beta$. The higher the ratio, the higher reward per unit risk.


#### Jensen's Alpha
Jensen's alpha measure the abnormal return that the portfolio earns after adjusting for beta.

You have to run a regression. $r_{i,t} = \alpha_i + \Beta_i(R_{m,t}-r_f)+\epsilon_{i,t}$

Calculate the difference between the portfolio against risk free. Calculate the difference between the market and the risk free rate. Run regression. Focus on the intercept. If the intercept is positive and significant, the fund has outperformed the benchmark. If negative, the fund underperformed.

#### Stock Splits
Stock splits are only cosmetic. Your value invested won't change, if you have 1000 shares at $20 but maybe they do a 2:1 stock split, you'd end up with 2,000 shares at $10.


## Historic Performance Analysis
Risk and return are related. Riskier stocks often shoot up...but they also fall. Bonds never have crazy returns, but they never go negative.
