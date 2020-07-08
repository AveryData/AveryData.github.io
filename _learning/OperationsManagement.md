---
title: "Operations Management"
excerpt: "You know, managing operations"
---

## Operations management is how to make stuff...on time, at low cost, high quality, meeting customer's demands
Could be making: Cars, gadgets, medicine, gas, education, food ... but doing so in high efficiency, at the lowest possible cost, increasing value. Taking inputs and making a more valuable output.Usually operations management and supply chain go hand-in-hand. Supply chain is raw material from the other getting processed by one customer who's output then becomes the input for the next customer until it ends with the end user and then back to the earth.

Want to see why operations management is important? [KFC ran out of chicken in the UK](https://money.cnn.com/2018/02/19/investing/kfc-chicken-shortage/index.html)

### Decisions could be long term, or short term

## Long Term
- What products do I want to make?
- What process should I use?
- Where do I want to be located?
- How much do I want to make?

## Medium Term
- Number of employees
- Inventory levels
- Order quantity and frequency

## Short Term
- What is the priority?
- Job scheduling?


## Queueing Theory(Ques = Lines)
We wait in line for any service: food, voting, check out, ect. If your line is too long, you run the risk of having a customer enter, see the line, say "no way I'm waiting that long", and decide to leave (and go to the competition).

- What is an acceptable waiting time for your customers
- Keep them entertained while waiting; divert attention
- Inform the customer of what to expect
- Keep employees that aren't serving customers out of site (looks bad/lazy
- Segment customers (TSA precheck, elite customers, VIP
- Have friendly employees
- Encourage customers to come during off-peak times (happy hours

### Timeline
1) Arrival
2) Queue
3) Service
4) Exit

Arrival Rate ($\lambda$): i.e. 1 customer every 6 minutes.

Exponential Distribution (when people show up distribution):
$f(t)=\lambda\cdot e ^{-\lambda \cdot t}$

Interarrival Rate (take a time T, how many people show up):
$P_T(n)=\frac{(\lambda T)^n\cdot e^{-\lambda T}}{n!}$

- Is there a limit to how many people can be in your queue? (How many cars till you block the road?)
- Number of lines?
- How do you decide who is serviced next? (FCFC (first come first serve), priority)

Service Rate ($\mu$): i.e. how many people can an employee service in a hour

- How many employees do you have?
- Phases: are there multiple steps?
- Any probability of reservice? (Going back in line?)

Service Distribution:
$f(t)=\mu\cdot e ^{-\mu \cdot t}$



## M/M/1 Model

Assumptions:
- Infinite Customer Population
- Random arrivals (Poisson)
- Unlimited queue length
- Single line
- FCFS
- Single channel (one server)(exponential)
- Single phase (only one step)


You only need to know the average arrival rate ($\lambda$) and the average service rate ($\mu$), and then you can really understand your system!

Utilization:
$\rho = \frac{\lambda}{\mu}$

Average # of Customers in the system:
$L_s = \frac{\lambda}{\mu - \lambda}$

Average # of Customers in queue:
$L_q=\frac{\lambda^2}{\mu(\mu-\lambda)}=L_s\cdot\rho$

Average time a customer spends in the system:
$W_s=\frac{1}{\mu-\lambda}$

Average time a customer spends in the queue:
$W_q=\frac{\lambda}{\mu(\mu-\lambda)}=\frac{L_q}{\lambda}$

Probability of *n* units in the system:
$P_n=(1-\frac{\lambda}{\mu})(\frac{\lambda}{\mu})^n=(1-\rho)(\rho)^n$


What's the probability of fewer than *N* people?
$P_n<N=\sum_n^N P_n$
