---
title: "Investing"
excerpt: "$$$"
---



## Returns
Return = Percentage change in stock price from one period to the next.

$Return = (Price_2 - Price_1)/Price_1$



But you have to worry about stock splits and dividends. You end up with this: $r_t = \frac {p_tf_t+d_t} {p{t-1}} -1$
With *d* as the dividend and *f* is an adjustment factor for stock splits.

### Compound Returns
Total return over an entire period. The cumulative effect a series of gains and losses has on our initial investment.

$CompoundReturn = \prod_{i=1}^{n} (r_i+1) - 1$

It's basically the experience of an investor over time.

### Risk
You can rarely just evaluate one return alone, you must compare to others and understand how risky they are. Returns are just part of the story.

Treasury Bonds are very safe...but some are high risk, high reward. How to do quantify these?

#### Standard Deviation
Standard Deviation is *total risk* including firm specific and market wide component.

$S = \sqrt{ \frac{\Sigma{(x_i-\bar x)}}{n-1}}$

Firm could be good or bad about the business specifically. But there are interest, global economy, pandemics ect.

If you hold a lot of stocks, the firm specific risk dissipates. We can separate the risk into a firm specific risk through linear regression.

$r_i = \alpha + \beta \cdot r_m + \epsilon$

With $r_m$ as the return of a broad stock index portfolio (think SP 500) and $\beta$ is a measure of a stock's sensitivity to overall market movements.

Then we use the $R^2$ to see what percentage of the fund's performance occurs due to the stock market. The higher the $R^2$ means the fund is more closely correlated with the stock market.

You can interpret $\beta$ as a measure of risk. High $\beta$ means high risk. The $\beta$ value will range from 0 (no risk) to 1 (risk of market) to $\infty$.


#### Drawdown
Drawdown (DD) is the cumulative loss since losses started. It measures peak-to-trough decline in your investment. It can be defined as:

$DD_t = \frac {HWM_t - P_t}  { HWM_t }$

Where $HWM$ is high-water mark and is the higest price a fund has achieved in the past.


#### $\beta$ Values
If you fit the returns of the company as a function of the returns of the market, you'll be able to evaluate how risky that stock is.

A **coefficient on the market input** explains the risk factors. A coefficient of 1 means as risky as the market. Greater than one suggests riskier than the market, while less than 1 risks safer than the market.

The $R^2$ also shows how much of the profits are influenced by the market. High $R^2$ means high correlation with the market.

#### Risk Adjusted Performance
Are our investments outperforming expectations?
$$ Abnormal Return = Actual Return - Expected Return $$

The easiest way can be to just to compare to an index.

#### Sharpe Ratio
The *Sharpe Ratio* is a measure of investment reward per unit risk; measure of award vs risk.

$Sharpe Ratio = \frac {R-R^f  } {\sigma (R-R^f) }$

where $sigma$ is the standard deviation of $(R-R^f)$. It can also be written as: $Share Ratio \frac {(R-R^f) } {\sigma_{r_x}}$

where $R$ is the return against $R^F$ is the risk free (think bond). High high Sharpe Ratios indicate better performance.

In R, using the library(PerformanceAnalysis), this can be calculated easily:
```{r}
SharpeRatio(berkshire_xts$BrkRet)
```

#### Treynor Ratio
Similar to Sharpe except the denominator is not $\beta$. The higher the ratio, the higher reward per unit risk.


#### Jensen's Alpha
Jensen's alpha measure the abnormal return that the portfolio earns after adjusting for beta.

You have to run a regression.

$r_{i,t} = \alpha_i + \beta_i(R_{m,t}-r_f)+\epsilon_{i,t}$

Calculate the difference between the portfolio against risk free. Calculate the difference between the market and the risk free rate. Run regression. Focus on the intercept. If the intercept is positive and significant, the fund has outperformed the benchmark. If negative, the fund underperformed.

#### Stock Splits
Stock splits are only cosmetic. Your value invested won't change, if you have 1000 shares at $20 but maybe they do a 2:1 stock split, you'd end up with 2,000 shares at $10.


#### Bid-Ask Spread
The difference of the highest price a buyer is willing to pay and the lowest price the seller. is willing to . See [investopedia definition.](https://www.investopedia.com/terms/b/bid-askspread.asp#:~:text=A%20bid%2Dask%20spread%20is,seller%20is%20willing%20to%20accept.)


## Historic Performance Analysis
Risk and return are related. Riskier stocks often shoot up...but they also fall. Bonds never have crazy returns, but they never go negative.

## Fees To Invest
There are usually costs to trading. It usually is in basis point costs (.01%). It can really take a cut on your return!

You need to think about being in a low-fee fund or high-fee fund.

Other fees:
- Commissions = fixed charge per fee
- Bid-ask Spread Cost = Quoted vs Immediate Purchase
- Delay Cost = slippage.

## Efficient Market Hypothesis
How easy is it to beat the market?

Even if you find a pattern you can exploit to make money in the market, exploiting that pattern diminishes that pattern. There's a lot of competition for people getting that pattern.

Stock prices should fully reflect all information, to some degree.

At long time horizons, most firms fail to beat the market.

However, if markets were perfectly efficient, there would be no incentive to collect information. Investors are also not really rational, and they make mistakes. This includes:
- Overconfidence: overestimate your own ability
- Loss Aversion: seek pride and avoid regret in decision
- Recency Effect: overemphasize recent information when making decisions.
- Anchoring: Taking action based on single fact that really isn't important in the decision.

### Weak Form
Today's stock prices reflect all the data (price and volume) of past  and that no technical analysis can effectively be utilized to aid investors. Can't predict future stock returns.

### Semi-Strong Form
All information that is public is used in the calculation of the stock's current price, and investors cannot utilize technical or fundamental analysis to get better gains. Can't make money by reading the Wall Street Journal, that information is already in the stock price.  

### Strong Form
Any information, both public and private, is completely accounted for in current stock prices. Even if only 1 person knows that information.

## Factors That Drive Returns
- Size
- Value
- Momentum
- Profitability
- Volatility

#### Size
Smaller firms have higher returns than larger, on average (usually about 3% per year since 1927).

Size is measured via market capitalization (price x shares outstanding)

#### Value
Inexpensive stocks tend to outperform expensive stocks. Value stocks have outperformed growth stocks by 4.67% / year on average.

Low B/M ratio means stock is expensive (growth)

High B/M ratio means stock is cheap (value)

#### Momentum
Stocks that have performed well over the past year will continue to perform well.

Past winners outperform losing stocks. Past winners outperform past losers by about 9.23%. Noe this is much more than the size or value effects.

#### Profitability
Profitable stocks to outpeform unprofitable stocks. Stocks with good performance outperform weak performance by ~3%/year.

#### Volatility
Also called risk effect. Low $\beta$ stocks outperform high $\beta$ at 12%/year. Low-risk stocks outperform risky stocks.

### Factor Regression

You can actually put these factors into a linear regression model to predict the return.

- Size = SMB (small minus big)
- Value = HML (High minus low)
- Momentum = MOM
- Risk = BAB (betting against Beta)
- Quality = QMJ (quality minus junk factor)

We can then learn from the coefficients!
- Positive coeff on SMB means fund is titled towards small cap stock
- Positive coeff on HML means more value stocks
- Positive coeff on MOM means high momentum stocks
- Positive coeff on QMJ means skew towards profitable stocks
- Positive coeff on BAB means fund skews to safe stocks
- Intercept tells us about the skill of the fund manager. Positive means outperformed. 

Example in R:
```{r}
q7 = lm(df$Brk_exret ~ df$Mkt_rf + df$SMB + df$HML + df$Mom + df$BAB + df$QMJ)
summary(q7)
```
