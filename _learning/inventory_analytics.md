---
title: "Inventory Analytics"
excerpt: "Keeping it all in stock through numbers"
---


# Types of Inventory
- Raw materials (wood, steel, oil, ect)
- Component parts (bought from somewhere else)
- Finished goods (what you made)

# Inventory Management
- Balance and matching of supply and demand in supply chain.  

# Reason to Keep Inventory
- Protect against spikes in demand
- Protects against supply disruptions (can't get stuff)
- Discount on high quantity order  

# Reason to Not Have Inventory
- Inventory can become obsolete
- Things can spoil
- Opportunity costs (money not liquid)
- Holding costs
    - insurance, security, spacing, warehouse costs


# Important Decisions
- HOW MUCH should we order?
- WHEN should we order more?


## How much should I order

Simple models start by assuming:
- *D*: Demand is known and constant
- *S*: The ordering cost is known and fixed.
- Instant replenishment
- No limit on order quality
- *H*: Annual holding cost per unit
- *Q*: The limit to which we build back up
- *P*: Price per unit

This would mean *Q* units would be bought. They would be depleted linearly till 0, then a price *S* would be paid to order more back up to *Q* level. Hence the average inventory would be $Q/2$

EOQ - Economic Order Quantity - The Size

$TotalCost=OrderingCost+InventoryCost$

$OrderingCost=NumberOrdersPerYear \cdot CostPerOrder$

In this case, $OrderingCost=\frac{D}{Q}\cdot S$


$InventoryCost=AverageInventory \cdot HoldingCostPerYear$

In this case $HoldingCost=\frac{Q}{2}\cdot H$


To find the minimum and optimal total cost, it is when holding cost equals ordering costs. Could also set the derivative to 0.

This leads to:

$Q^{*} \sqrt{\frac{2SD}{H}}=EOQ$

Expected orders: $N=\frac{D}{Q^*}$

Expected time between orders: $T=\frac{NumberOfDaysPerYear}{N}$



## How Can We Account for a Quantity Discount?

$Expand Equation for Total Cost (ETC) = \frac{D}{Q}\cdot S + \frac{Q}{2}\cdot H + P\cdot D$

This will allow for discount in price/unit on large quantity orders. Discounts encourage to hold more inventory.


## At What Point do you reorder?
In real life, a instant replenishment time is unrealstic and impossible. Hence, there are lead times.

- *L*: Lead time in days
- *d*: average daily demand


ROP = Reorder Point.

$ROP = d\cdot L$

But what if demand is not constant? How does ROP change?

Well we can model it with a probability distribution. Will add a buffer of extra inventory and call it our safety pile. This size will depend on demand uncertainty, the penalty of running out, lead time.

Use a normal curve for the probability of having enough stock.

The safety stock can be lead to be represented as

$Safety Stock = Safety Factor \cdot STD(Lead Time Demand)$

The safety factor is just the Z from the % you don't want to be out of stock.

Instead of standard deviation of the lead time, is set to be

$STD(LeadTime) = Z\cdot \sigma_D \cdot \sqrt{LT}$

Hence we end up with $ROP = d\cdot L + SS$
