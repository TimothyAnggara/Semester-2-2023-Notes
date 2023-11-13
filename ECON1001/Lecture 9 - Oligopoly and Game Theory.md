## Introduction
- Oligopoly is a market that contains a small number of firms
	- Soft drink market, computers, phones, cars, banking, supermarkets, etc.
	- Strategic interaction between firms is critical
		- E.g. If one firm drops their price, other firms' may be pressured to follow
- Game theoretic models are used to analyze strategic interactions in these markets

## Characteristics of an Oligopoly
- Few sellers and many buyers
- **Price-making firms**
	- Because there are only a small number of firms, each firm retains the power to set its own price
- **Barriers to Entry**
	- Entry into market is difficult as there are significant barriers to entry
- **Possible product differentiation**
	- May be differentiated or not, depending on the market

### Game between Delivery Platforms
- Assume there are only two delivery platforms in University City - UniEats and DeliverDash
- Given current cost and market demand, suppose that:
	- Price is either high or low and the platform can choose its price knowing which price its competitor will choose
	- If both platforms charges high, profits are 100 each
	- If both platform charges low, profits are 60 each
	- If one platform charges high and the other charges low, profit of the low-price is 120 and profit of high-rice is 0
- What would the two platforms charge?
	- We would charge low because it is the *dominant strategy*.
	- If the opposing firm charges high, you can charge low to make more money
	- If the opposing firm charges low, you also charge low because if you charge high you get 0 profit

## Simultaneous-Move Fames
- The example with UniEats and DeliverDash is known as a *Simultaneous-move game*
- A game is any strategic situation where there are two or more players
	- Each player can take two or more action and the payoff that each player receives depends on all actions taken by all players
- A Simultaneous-move game is a game where all plays choose their actions simultaneously or when each player choose their own actions without knowing the actions chosen by the other
- We can describe simultaneous-move games in their normal forms (A payoff table)
![[Pasted image 20231009114244.png]]
- If DeliverDash chooses a high price, UniEat's best response is low price
- If DeliverDash chooses a low price, UniEat's best response is low price
- If UniEats chooses a high price, DeliverDash's best response is low price
- If UniEats chooses a low price, DeliverDash's best response is low price

- Low price here is  the strictly *dominant strategy* where for each platform "low price" yields a strictly higher payoff than "high price" no matter what the opponent does

### Nash Equilibrium
- A pair of action is a Nash Equilibrium (NE) if both players are playing their best response
	- In the example above, (Low Price, Low Price) is the unique Nash equilibrium in this game even though their payoffs would be higher if they both choose (High price, High Price)
- In a Nash equilibrium, neither player has a strictly profitable unilateral deviation to a different action
	- If my opponent does not change their action, I am never strictly better off if I change my action
	- Given the current situation, no player has a more profitable action that they can take

### Prisoners' Dilemma
- Two criminals are separately questioned about a crime and they have no way communicating with each other
- If both stay silent, they will be criminated with one year of prison
- If one gives evidence and his partner stays silent he will go free but his partner will be imprisoned for 3 years
- If both gives evidence, both will get two years
![[Pasted image 20231009120001.png]]
- In this scenario, the Nash Equilibrium choice is the scenario where they both confess.

### Can firms avoid the Prisoner's Dilemma
- A possible way out is being able to infinitely or indefinitely interaction with each other
	- Firms often face each other many times
	- Always a positive probability that firms will face each other again tomorrow / next year and it is possible to "punish" a competitor in the future if they do not cooperate today
	- Future cost of punishment is sufficiently high, firms will avoid being too competitive in the short run
### Coordination Game
- Some games may have more than one Nash Equilibrium
- Daisy and Jay are a couple.
	- Daisy loves boxing, and Jay loves the opera. 
	- For their weekend activity, Daisy prefers to go boxing, and Jay prefers to go to the opera
	- But both of them prefer to be together and not apart.
![[Pasted image 20231009121227.png]]
- In this scenario, (Opera, Opera) and (Boxing, Boxing) are both Nash Equilibria
- Between these two Nash Equilibria, we cannot say which is more likely
### Application of the Coordination Game
- Say two firms decide where to set up their new restaurants
	- Suppose there are two possible locations, A and B, and they want to set up at different locations so that they will get all the customers at that location
	- If the firms set up business in the same location, they will have to share the customers at that location
![[Pasted image 20231009122830.png]]
### Lessons from coordination game on Product Choice
- Applications of the Coordination game illustrate that sometimes firms in an oligopoly prefer to offer products that are similar to those of their rivals and sometimes different from their rivals
- Which case applies depends on the specifics of the market of interest
- Unlike markets for perfect competition, monopoly, or monopolistic competition, there is no "one formula fits all" for oligopoly
### Avoiding Coordination Failures

## Sequential Games
- In many economic situations, one player moves first and the second player makes their choice after observing the choice of the first player
	- These games are called sequential games
- Often convenient to represent games in the form of a 'game tree'
![[Pasted image 20231009123402.png]]
- Given this scenario, we can solve this game with backwards induction
- In each scenario that developer 2 may encounter, what would be developer 2's best response?
- With backward induction, we predict that both developers will choose Platform Y
	- The prediction using backward induction is called a subgame perfect equilibrium
![[Pasted image 20231009182645.png]]
## First-Mover Advantage
- IN the platform choice game, when two developers move simultaneously, there are two Nash equilibria
	- (X,X) and (Y,Y)
		- Developer I prefers the (Y,Y) equilibrium
		- Developer II prefers the (X,X) equilibrium
- When they sequentially move and have developer I move first, the unique SPE outcome is that equilibrium outcome that is preferred by developer I
	- (Y,Y)
	- This is called **first-mover advantage**
- By moving first, the first-mover essentially chooses the equilibrium that it prefers
## Second-Mover Advantage
- Sometimes it is better to be a follower than a leader
	- Many of the most successful firms in an oligopoly market are followers
		- Think of Elon Musk, he didn't create the first electric car but he is very successful now
- Sometimes it is better to take advantage of a rival's investment or innovation 
	- This is known as a second-mover advantage
	- 