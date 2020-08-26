---
title: "Control Charts"
excerpt: "iS ThIS DAta OuT oF CoNTroL!!"
---

Control charts try to show when the mean or variance of a population has changed.An ongoing hypothesis test that the variance and mean will not change.

Run charts where the x-axis is successive measurements (usually time or test #). Often there are upper control limits, lower control limits, and a mean line, laid on top of the raw data.

The upper and lower control limits are based on random variation based upon the normal curve. If a point crosses a control limit, it is said to be out of control and special cause of variation. High points could also be worthy of looking.

The upper and control limits are usually set based on a standard deviation. 3SD means that 99.73% chance of all values following within that range due to randomness.

### R Charts
These are control charts based on ranges of average samples repeated.

$UCL = \bar{X} + A_2 * \cdot \bar{R}$

$LCL = \bar{X} - A_2* \cdot \bar{R}$

Where $R$ is the range: Maximum - minimum.

A2 comes from a mean factor, but often is represented as D4 for Upper Limit and D3 for Lower Limit.


### Process Capacity Ratio (Cp)

$Cp = \frac{UpperSpec - LowerSpec}{6\cdot \sigma}$

If $Cp>1$ it indicates the process is capable.

Six Sigma equates to a $Cp>2$

This value only looks at the spread and not how well a process is centered on a target value.


### Proces Cabability Index (Cpk)
Minimum of two equations:
$Upper Spec - \frac{\bar{x}}{3\sigma}$ and
$\bar{x} - \frac{Lower Spec}{3\sigma}$

Gives the proportion of variation between the center of the process and the nearest spec limit.

$Cpk = 1$ mean process meets specifications

$Cpk > 1$ process is better than spec limits requires

$Cpk < 1$ process does not meet specifications


### Stability Index
Close to 1 means the within variance is close to the overall variance.

[See this link](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.504.1926&rep=rep1&type=pdf)


### Western Electric Rules
[See this wikipedia](https://en.wikipedia.org/wiki/Western_Electric_rules)



## Multivariate Control Charts
When you have hundreds, thousands, or even dozens of variables, control charts become too complex.
