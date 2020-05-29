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

+ Predictor variables are assumed to be linearly independent
+ The value of Y must be linearly associated with the x's

## Things To Do Before Regression
Before you do regression, it's important to do EDA (exploratory data analysis). This could include making histograms, making scatter plots, and finding correlations.

You'll surely want to view scatters of all the x's against the y's to see for a good fit, and any shapes (hopefully line shapes)!

### Correlations
Correlation coefficient only captures *linear* relationships

## Variable Selection
Variable selection should probably be its on section...so maybe I will make it that eventually (copy to new section reminder!)

## Fitting Methods
- OLS - Ordinary Least Squares
- Generalized
- Step-wise
- Maximum Liklihood

## Categorical Data
Not all data is continuous and quantitative, it is often categorical. Think 'gender', 'Age: Old, Young, Middle'. 'Pet: Dog, Rabbit, Cat'.

If you have 3 categories, you will need two indicator or dummy variables. The base (or reference) case, with both dummy variables set to 0, is the reference group to compare to the complementary dummy variables. For the age, we can make MiddleAged and OldAged. The OldAged would be set to 1 if the person is old. Both would be set to 0 if the person was young. The intercept then represents the other variable of young (because MiddledAged and OldAged would be 0, hence only int is left (corresponding to young)). The coefficient if OldAged would be the **increase** of a young customer (because you still have the intercept)

## Logistic Regression
A dependent variable that has vinary values (pass/fail or true/false or 0/1).

Some examples might be:
- Will the student get an "A" in the course?
- Will this product pass all quality specs?
- Will this thing survive? Titanic for example.
- Will the customer purchase the item?
- Will the party make their loan payments?

The odds (remember $\frac{p}{1-p}$) can be simply written as $exp(linear equation)$. If you take the log of both sides you can get this: $$ log(/frac{p}{1-p}) = linear equation $$

This constitutes the log odds of this event happening. This makes it so the range now is between 0 and 1.

### Interpreting coefficients
As x increase by 1 unit, the log odds increase by the coefficient. Or you can find the odds change by taking $exp(coeff)$. This apparently approximates to 100*coeff%.


## Model Validation

## Interaction Terms
Something like $x_1*x_2$

## OLS - Ordinary Least Squares
Fit a line that minimizes sum of squared errors (SSE).

$$ SSE = \Sigma (y-\hat{y})^2 = \Sigma (y-(\beta_0+\beta_1 \cdot x_i))^2$$

Total Deviation is the difference between observed value $y_i$ and mean value $\hat{y}$.

Total Sum of Squares (SST): The sum of sum of squared errors (SSE) plus sum of squares regression (SSR). $\Sigma (y_i - \bar{y})^2$

Sum of Squares Regression (SSR): Sum of squared differences between **mean response** and  predictions. $\Sigma (\bar{y} - \hat{y})^2$

Sum of Squared Errors (SSE): Sum of squared differences between **observations** and predictions. $\Sigma (y_i - \hat{y})^2$




## $R^2$
The coefficient of determination: measure of overall strength of the relationship between the dependent variable and independent variables.

$$R^2 = 1 - \frac{SSE}{SST} = \frac{SSR}{SST}$$

Or simply stated, it is the amount of variance explained by the model, divided by the total variance. How much variance does the model account for?

Never decreases with additional independent variables added Ranges between 0 and 1.

IN THE FUTURE ADD EXAMPLES OF R^2 = 1 and R^2 = 0

### Adjusted $R^2$
Adjusted $R^2$ ensures you never account for the inflation of number of predictors. Adds a penalty for the number of terms in model

$$R^2 = 1 - \frac{ \frac{SSE}{n-p-1} }{ \frac{SST}{n-1}}$$




## Simple Linear Regression
$$ y = \beta_o + \beta_1\cdot x + \epsilon$$

We use one variable and a constant to model the target variable. There is error included as the fit is likely to be not perfect.

We don't know $\beta_o$ or $\beta_1$.

## Significance
The *t* value is: $t=\frac{Coefficeint}{Standard Error}$

The *p* value is the probability of finding a *t* value so large, given the null hypothesis that the coefficient should be 0. A low *p* value indicates this coefficient really shouldn't be zero and hence is significant. If the *p* value is large (depends on your confidence level, but say greater than 0.05), that coefficient is **not** significant.

The *F-Test* is a way to check if the model is statistically significant. The *F-Statistic* is the probability that the model coefficients are equal to 0. It is defined as the ratio of means regression sum of squares divided by the mean error sum of squares. It ranges from 0 to an arbitrary large number.

$$ F = \frac{ \frac{SSR}{p}  }  { /frac{SSE}{n-p-1}   } $$

## Intepretting Coefficients
If $\beta_1$ has a value of 300, that means for every one unit change in the explanatory variable associated with $\beta_1$, we can expect an estimated 300 increase in the dependent variable.

These coefficients must be interpreted assuming all other variables are held constant...this invokes a lack of colinearity between input variables. See the section on multi collinearity for further explanation.

## Multi Colinearity
Two or more explanatory variables are highly correlated with one another. Makes explaining coefficients magnitudes very difficult. Violates one of the assumptions that input terms are independent. Only use one variable to represent multiple correlated variables. There are several techniques to do this automatically for you...see stepwise, generalized regression, or even PCA / Factor Analysis.

VIF - Variance Inflation Factors -- Predict a given input variable with all other input variables...how well do you do? If VIF > 5, multi colinearity is present.

Regression coefficients get messed up...they could have large variance, wrong sign, 0, explained incorrectly.  

## Outliers
Outliers are points that have a y value far from the predicted y. They can be identified by looking at the residuals plot. It may need to be removed.

You can identify outliers by **Cook's Distance**

### High Leverage points
A predictor value outside the observations. If it is removed and the model changes significantly, it is thought to be high leverage.

## Things to check

- Is the relationship linear?
Scatter Y against all x's...see if it is well correlated and linear.
##### If the variable is nonlinear
- Can you use a non-linear relationship with higher order terms? I.e. add a square term?
    - Cannot read coefficients because the slope is not constant, with every change of x, your slope is changed.
- Use variance reducing  transformations (such as log)
    - Note that logs don't work for negative numbers.
    - Did you try logging the x variable?
        - Coefficients can be interpreted as a 1% increase in X corresponds to a coefficient/100 change in Y.
    - Did you try logging the y variable?
        - Coefficients are interpreted as the dependent variable chances by coeff * 100 for a one unit increase in the X.
    - Did you try loging both x and y?
- Did you possibly leave any important predictor variables out?

- Are the residuals (errors) normal?
Make a q-plot or histogram of the residuals. If a histogram, does it look normal? If q-q plot, is it linear? Does it have tail deviations at the ends? That may suggest outliers.

- Are the errors correlated?
Residuals vs fitted values. We want to see no patterns, otherwise we have correlated errors (autocorrelation). This would violate independence and constant variance assumptions. Durbin-Watson test can be used to detect autocorrelation in linear model.

## Variable Transformations

## Categorical Variables

## Linear Regression in R

To fit a linear regression:
```R
model = lm(y ~ x1 + x2)
```

To see the summary:
```R
summary(model)
```
You'll see this includes (among others) the coefficients, the intercept, as well as the significance probabilities, and the $R^2$.

Predicting using linear regression in R:

```{r}
# Create New Data Point
new_airbnb = data.frame(x1 = 19, x2 = 43)
# Make prediction
predict(model,new_airbnb,type="response")
```
