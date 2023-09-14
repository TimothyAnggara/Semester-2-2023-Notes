## Elasticity
Elasticity is the responsiveness of one variable to a change in another variable
- How does **demand** respond to changes in **consumer income**
- How does **supply **respond to a change in **input prices**

Elasticity concepts considered & relevance
- Own price elasticity of demand
- Price elasticity of Supply
- Cross-price elasticities of demand
- Income elasticity of demand

We are often interested in measuring how a change in one variable affects another and to measure
- An issue with using quantitative changes is that different markets use different units of measurement(kg, liters, ounces) and its own price level (cents, millions)
- Elasticity is a way we can compare quantitative changes across different situations by looking at **proportional** or **percentage** changes

- Elasticity measures how one variable (y) changes in another variable (x)
	- When we increase x by a certain amount, does y change by a small or large amount
	- We can calculate elasticity by dividing the percentage change in y by the percentage change in x
$$
\epsilon = \frac{\% \triangle y}{\% \triangle x}
$$
- This says that for 1% change in x, there will be a $\epsilon$ % change in y

### Elasticity - Proportional Change
There are two classic methods to find elasticity, the **point method** that uses the** initial values** and the **midpoint method** that uses the **averages of the initial and final points**

### Point Method
We use the point method when we are interested in the elasticity around a particular outcome
	- What is the elasticity on a demand curve around the point $(Q_1, P_2)$
In these scenarios we use the point method for calculating elasticity
$$
\epsilon = \frac{\triangle y / y}{\triangle x / x} = \frac{\triangle y}{\triangle x} \times \frac{x}{y} = \frac{dy}{dx} \times \frac{x}{y}
$$
### Example
Suppose that the demand for forks is given  by $Q = 100 - 2P$ and the price of forks is P = 30. What is the price elasticity of demand at this price?

$\frac{dP}{dQ} = -2$ and our $p = 30$so $Q = 40$ if we plug it into our demand function. 
$$
-2 \times \frac{30}{40} = -1.5
$$
- The interpretation is that if the price of forks increase by 1%, the quantity demanded by forks falls by 1.5%

## Midpoint or Arc Elasticity
We use the midpoint method when we are interested in elasticity when moving from one point to another
	- When the price of a good changes from $P_1$ to $P_2$ which causes quantity demanded change from $Q_1$ to $Q_2$
$$
\epsilon = \frac{\triangle y / y^m}{\triangle x / x^m} = \frac{\triangle y}{\triangle x} \times \frac{x^m}{y^m} 
$$
Where: $y^m = \frac{y_1 + y_2}{2}$ and $x^m = \frac{x_1 + x_2}{2}$

### Midpoint Elasticity: Example
When the price of spoons is $10, the quantity demanded is 50 units.  When the price increases to $20, the quantity demanded falls to 30 units

To calculate elasticity, we need to find the average of price and quantity:
- $P^m = \frac{20-10}{2}$ and $Q^m = \frac{50 - 30}{2}$
We also need to know $\triangle P$ and $\triangle Q$
- $\triangle P = 20 - 10 = 10$
- $\triangle Q  = 30 - 50 = -20$
Hence,
$$
\epsilon = \frac{-20}{10} \times \frac{15}{40} = -0.75
$$
- When the price of spoons decrease by 1%, the quantity demanded of spoons falls by 0.75%
- Note that epsilon is always negative in this context because of the law of demand
	- When price increases, demand decreases
## Intermezzo: Own Price Elasticity of Demand
Why is it useful to understand the responsiveness of quantity demanded to a change in price?
- Business might be interested in the effect of a price change on quantity sold, revenue, and profits
- Governments may want to know how (consumer or firm) behavior changes when a specific policy is put in place

### Interpretation of Price Elasticity Demanded
If elasticity is 0, demand is perfectly inelastic
- For a 1% change in price, there is no change in the quantity demanded
- Quantity demanded is not responsive to the change in price
If the elasticity is between -1 and 0, the demand is inelastic
- For a 1% change in price, the resulting change in quantity demanded is less than 1%
If the elasticity is -1, demand is unit elastic
- For a 1% change in price, the resulting change is a 1% change in quantity demanded
If elasticity is less than -1, the demand is elastic
- A 1% change in price results in a change in quantity demanded larget than 1%
- Very responsive to price
If elasticity is negative infinity, the demand is perfectly elastic
- For a small increase in price, quantity demanded will drop to 0

### Perfectly Inelastic and Elastic Demand
![[Pasted image 20230904120100.png]]

### Price Elasticity of Demand Along a Straight-Line Demand Curve
The price elasticity of demand depends on the slope of the line, but also the reference point on the curve to calculate elasticity

Recall that the formula for elasticity is:
$$
\epsilon = \frac{\partial Q}{\partial P} \times \frac{P}{q}
$$
Given that the the demand curve is linear, our gradient will be constant hence, elasticity varies depending on quantity and price. For every linear demand curve, there is:

- Inelastic Section - when quantity is high and price is low
- Unit Elastic - Middle of the demand curve
- Elastic Section - When quantity is low and price is high
![[Pasted image 20230904120346.png]]

## Elasticity and Revenue
We can determine how total revenue in the market will change as price changes from the elasticity of demand
- Recall that the demand curve is the quantity demanded in the market (q) depends upon the market price (P), hence we can write the quantity demanded as a function of price q(P)
- Total revenue as a function of P is: $TR(P) = P \times q(P)$

Total revenue can be calculated by multiplying the price and quantity or graphically, it is the area of a rectangle below the demand curve
![[Pasted image 20230904121544.png]]

If price increase which results in quantity demanded decrease, TR:
- Decreases when elasticity is elastic
- Constant when elasticity is equal to one
- Increases when elasticity is inelastic

If price decreases which results in an increase in quantity demanded, TR:
- Increases when elasticity is elastic
- Constant when elasticity is equal to one
- Decrease when elasticity is inelastic
![[Pasted image 20230904185037.png]]

### Changes in Revenue and Elasticity
- The intuition for the result is:
	- If demand is **elastic**, a 1% increase in price will cause a greater than 1% fall in quantity demanded which means that the increase in price will cause a greater decrease in quantity demanded thus causing total revenue to fall
	- If the demand is inelastic, the 1% increase in price will cause less than 1% to fall in quantity demanded thus the increase in price outweighs the decrease due to quantity demanded which will cause total revenue to increase

We can create a differential equation on how the total revenue reacts to changes in price through this formula
$$
\frac{dTR}{dP} = q(1 + \frac{P}{q}\frac{dq}{dP}) = q(1+\epsilon)
$$
We can look at this formula and we can see that TR only increases when $\epsilon$ is inelastic i.e. between 0 and -1

- On the **elastic** part of the demand curve, the price needs to be lowered to increase total revenue
- On the **inelastic** part of the demand curve, the price needs to be raised to increase revenue
- Thus total revenue is maximized when demand is **unit-elastic**

## Determinants of Price Elasticity of Demand
- Substitutability with other goods
- Luxuries vs. Necessities
- Proportion of income spent on good
- Timeframe

## Elasticity of Supply
Price elasticity of supply, $\epsilon_s$, measures how sensitive the quantity supplied of a good $(q_s)$ is to change in price (P)
- A 1% change in price causes a $\epsilon_s$ % change in quantity supplied of a good

- Midpoint Method

$$
\epsilon_s = \frac{\triangle q_s}{\triangle P} \frac{P^m}{q^m_s}
$$
- Point Method
$$
\epsilon_s = \frac{\triangle q_s}{\triangle P} \frac{P}{q_s}
$$
The elasticity of supply is typically positive due to the law of supply
- When price increases, supply also increases

- If $\epsilon_s = 0$, then it is perfectly inelastic 
	- For a 1% change in price, there is no change in quantity supplied
	- Supply curve is vertical
- If $\epsilon_s$ is between 0 and 1, then it is  inelastic
	- For a 1% change in price, the change in quantity supplied is less than 1%
- If $\epsilon_s = 1$, then it is unit elastic
	- For a 1% change in price, there is a proportionate 1% change in quantity supplied
- If $\epsilon_s > 1$, then it is elastic
	- For a 1% change in price, the change is quantity supplied is more than 1%
- If $\epsilon_s = \infty$ then it is perfectly elastic
	- For a small decrease in price, the quantity supplied will drop to zero 
	- For a small increase in price, the quantity supplied will increase from 0 to $\infty$ 
	- If the price of a good falls below a certain price, firms will stop supplying the product
	- The supply curve is horizontal
![[Pasted image 20230904191558.png]]

## Cross-Price Elasticity
Sometimes we are interested in the relationship between the quantity demanded of one good and the price of another related good

This relationship can be examined using the **Cross-Price Elasticity** which measures how sensitive the quantity demanded of Good A $(Q_A)$ is to changes in price of Good B $(P_B)$
- Point Method
$$
\epsilon_{AB} = \frac{dQ_A}{dP_b} \times \frac{P_B}{Q_A}
$$
- Midpoint Method
$$
\epsilon_{AB}= \frac{\triangle Q_A / Q_A^M}{\triangle P_B/P^M_B}
$$

### Example
- Price of teabags is $4 per box and Candice can sell 100 liters of milk
- If the price of teabags at $8 per box Candice can now only sell 60 liters of milk

In this situation we can use the midpoint formula
- $\triangle Q_A = -40$
- $\triangle P_B = 4$
- $Q_m^A = 80$
- $P_M^B = 6$

Substituting the values into our formula
$$
\epsilon_{AB} = -40/4 \times 6/80 = -0.75
$$
So now that we have our cross-price elasticity, how can we interpret this value?
- If $\epsilon_{AB} < 0$, the price of Good B is associated with a fall in the quantity demanded of Good A
	- Meaning that Good A and Good B are complements
	- Goods that are likely consumed together (Bacon & Eggs)
- If  $\epsilon_{AB} > 0$ an increase in price of Good B is associated with a rise in quantity demanded of Good A
	- Meaning that Good A and Good B are substitutes
- If $\epsilon_{AB} = 0$, an increase in Good B is not associated with any in the quantity demanded of Good A
	- Meaning that they are Independent

### Intermezzo: Cross-Price Elasticity
Its useful to understand the responsiveness of quantity demanded to a change in the price of a related good because we can explore relationships that different goods can have

## Income Elasticity
Demand for a good may also depend in part on a consumer's income

Income elasticity (η) measures how sensitive the quantity demanded of a good changes in income

- Point Formula
$$
\frac{dQ}{dY} \times \frac{Y}{Q}
$$
- Midpoint Formula
$$
\frac{\triangle Q / Q^m}{\triangle Y / Y^M}
$$

- When income η < 0, demand increases when income rises
	- Good is called an inferior good
- When income η = 0, demand is invariant to changes in income
	- Good is called an neutral good
- When $0 < η \leq 1$, If incomes rises 1% demand for the good increases by less than 1%
	- Good is called a normal good
- If η > 1, then the income raises by 1%, the demand for the good increases by more than 1%
	- Good is called a luxury good


