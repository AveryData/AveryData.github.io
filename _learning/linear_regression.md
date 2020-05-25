---
title: "Linear Regression"
excerpt: "$ y = \beta_o + \beta_1\cdot x $"
---



# Linear Regression


## Regression In General

Regression is used to quantifiably model the relationship between a target variable and inputs. The target variable is often called the response variable, dependent variable of 'y'. The inputs are often called the predicting variables of 'x'.

Linear regression is 1) Easy to Understand 2) Easy to implement 3) Works well for lots of things.

**Linear models aren't reality, but they're useful pieces of advice**

It can be used for:
- prediction of response variable in different scenarios
- modeling the relationship between response variable and explanatory variables
- testing hypothesis of assocation relationships

Regression can be diagnostic, predictive, or prescriptive.
- Diagnostic: Understanding the relationship and why
- Predictive: What will happen
- Prescriptive: What to do to make something happen

Regression ranges in various forms, some of the most common being: simple, multi-variate, and polynomial.

## Examples
- **Used Car Price** - how many miles, year, condition, model
- **House Price** - year built, how many bed/bath, amenities, neighborhood
- **Salary of Employee** - years experience, location, degree
- **Yield of Reactor** - operating conditions, feed qualities
- **Distance Dog Walked** - Day of week, weather, month

## Assumptions
1. Mean Zero Assumption - expected value of errors is 0
2. Constant Variance - Model shouldn't be more accurate for sections of population than other sections
3. Independence - Knowing you're underpredicting one y, shouldn't tell you anything about another, especially important in time series
4. Normally distributed errors - should be independent and identically distributed (i.i.d.) random variables with normal distribution of mean 0 and standard deviation

## Things To Do Before Regression
Before you do regression, it's important to do EDA (exploratory data analysis). This could include making histograms, making scatter plots, and finding correlations.

### Correlations
Correlation coefficeint only captures *linear* relationships

## Variable Selection
Variable selection should probably be its on section...so maybe I will make it that eventually (copy to new section reminder!)

## Fitting Methods
- OLS - Ordinary Least Squares
- Generalized
- Step-wise
- Maximum Liklihood

## Logistic Regression
A dependent variable that has vinary values (pass/fail or true/false or 0/1)

## Model Validation

## OLS - Ordinary Least Squares
Fit a line that minimizes sum of squared errors (SSE).

$$ SSE = \Sigma $$

Total Deviation is the difference between observed value $y_i$ and mean value $hat{y}$.


## $R^2$





## Simple Linear Regression
$$ y = \beta_o + \beta_1\cdot x + \epsilon$$

We use one variable and a constant to model the target variable. There is error included as the fit is likely to be not perfect.

We don't know $\beta_o$ or $\beta_1$.
