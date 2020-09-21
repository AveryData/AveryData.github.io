---
title: "Financial Management"
excerpt: "Managing Money"
---

## What is Financial Management?
Financial management is helping corporate investment decision making. The main question at hand being, how do you allocate the internal capital for future growth. It looks at investment alternatives, estimates a project's estimated cash flow, evaluates stock and firm values, measures risk and expected return, ect.

Some questions financial management considers: What should a company do moving forward? Where they should open up? Where should they shut down? Who they should hire?

Their goal is is maximize shareholder wealth. When shareholder's are happy, everyone is happy.


## What makes a company successful?
1) Taking care of customers on a repeated basis.

2) Create cash flow, continuous employment, decrease cost of capital, managing debt properly



## Capital Budgeting
Capital Budgeting is the process of determining how much to invest and what to invest in.

You have to 1) Identify: find opportunities and generate investment proposals 2) Evaluate: estimate the projects cash flows and discount rate (cost of capital) 3) Selection: Choosing criterion for acceptance and rejection. Often Net Present Value, Payback Period, and Rate of Return. 4) Implementation: Monitor your decisions, does it make sense to continue?, establishing an audit and a follow up procedure.


## Time Value of Money
Money coming you to the future is worth less compared to receiving it today.

You always have to include the interest, and maybe even opportunities cost.

###### Future Value

$Future Value = Present Value \cdot (1+i)^n$

Excel even has a Future Value function (FV).

###### Present Value
How much money would $1,500 in 5 years be today?

$$Present Value = Future Value \cdot (1+i)^{-n}$$

This also has a function in Excel as well, PV.

###### Payment
How much money a year would you need in order to match present value over time.

$$Anual Payments = \frac{r\cdot PV}{1-(1+r)^{-t}}$$

Use the PMT function in Excel.


###### Net Present Value (NPV)
Net present value is a way to evaluate long-term investments. It is seen as the greatest way to quantify a project.

Add all your cash flows.

$$NPV = C_0 + \frac{C_1}{1+IRR} + \frac{C_2}{1+IRR^2 }+ \frac{C_3}{1+IRR^3} + ...$$

Often the first few cash flows are negative (you have to invest).

If NPV > 0, accept the project. If NPV < 0, reject the project.

In Excel, the NPV(C's)+Original.

Crossover Rate: $NPV_A=NPV_B$

###### Internal Rate of Return (IRR)
IRR is the discount rate or interest rate that is the **minimum** return that would make the NPV of 0.

As long as the IRR is higher than the capital percentage cost. But choose the largest if mutually exclusive.

Activities like mining where end of project have a big cash requirement, you can have two IRR's because the NPV is concave.

Also, would you want 100% of $1 or 50% of $1,000. The answer: 50%! But IRR suggests differently.

IRR and NPV usually give the same answer, unless non-convential cash flows (sign changes more than once) and mutually exlusive proejcts.


###### Payback Period
The payback period is how long it'll take to cover the initial investments.

Penalizes long-term projects that take awhile to get off the ground.

$$PaybackPeriod=OriginalCost/CashInflows$$


###### Probability Index (Benefit Cost Ratio)
Highest PI is good, but bigger than 1 is profitable.

$$PI = \frac{CF_o+NPV}{CF_0}$$


## Cash Flows
Cash flows are recorded when the money actually moves, not when the accountant using accrual concepts says they occur.



**Sunk Costs** are not relevant for present decisions. You've already spent it and won't affect the project.

**Test Marketing Costs** are expenses expended. Money to investigate a project.

**Erosion Costs** are cash flowed from sales/existing customers from other projects to a new project

**Opportunity Costs** are lost revenues from alternative use of the assets.

**Depreciation** is a non-cash expense.

**Working Capital** is the amount you spend on things like initial inventory, so it is cash outflow at the beginning but can turn back into cash inflow on the way back.


**After Tax Cash Flows**
$$ATCF=(Revenue - Costs - Depreciation)\cdot (1 - Tax) + Depreciation$$


## Inflation
Inflation can be very large, especially in international business where inflation isn't as stable as it might be in the United States. It's also important for large inflation or longer time horizons.

### Nominal Rate of Return
Nominal rate is the percentage change in the amount of money you have.

Cost of capital is expressed in nominal terms.

### Real Return
Real return is the percentage change in the amount of stuff you can actually buy.

### Relationship
The relationship is often called *The Fisher Effect* and is demonstrated by

$$1+R=(1+r) \cdot (1+h)$$ where *R* is the nominal return. *r* is the real return, and *h* is the inflation rate.

We can also say that approximately:

$$R=r+h$$

## Simulations
If you don't know the numbers, run a simulation! You can have your variables follow distributions and repeat it thousands of times.


## Stock Evaluation
Stock ownership is pretty much cash flows from dividends and capital gains.

### Zero Growth Model
The dividends remain the same forever. This is pretty much the preferred stock. The future value of all future dividends. Given by $P_0=\frac{Div}{R}$ where R is *equity model*.

### Constant Growth Model
The dividends will grow at a constant rate, *g*. Simplifies to $P_0=\frac{Div_1}{R-g}$.

### Differential Growth Model
Dividends will vary for awhile and

#### R
R is the discount rate and can be broken into two parts: dividend yield, growth rate (in dividends)

## OTHERS

Undiscountod Payback Period

IRR
IRR on combined investment

NPV

Which investment should you take?

Mutually Exclusive

###### Independent Projects
Independent projects are those that acceptance/rejection decision are made independently of other projects. Each one can be decided by their own merits.

###### Mutually Exclusive
You can't accept both projects. It is one or the other.
