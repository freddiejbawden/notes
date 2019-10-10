# Unit 5 - Ant Colony Optimization

ACO aims to emulate the behaviour of pheramones in ant colonies. The more often an ant travels a path to a point, the higher the probability later ants will follow. 

Another important note is that we decay the trail over time to help the ants forget suboptimals olutions

## Artifical Ants

In ACO Pheromones cannot be the only source of information. Their distribution will need to depend on another heuristic, for this reason ACO has not been very successful in the real world. 

### Example: Travelling Salesman

* Each ant builds its own route
* Each ant chooses a town to go to with a probability that is a function of; the town's distance and the amount pheromone on the connection. 

This yields a network of nodes connected with the probabilities that an ant should follow it attached. We use this to derive our solution.

## Probability Rule

ACO uses a compound probability of the pheromone and random chance to explore and exploit regions. 

!["Probability Rule"](assets/probabilityrule.png)

The strength of the pheromone `τ(i,j)` is the likelihood of j following i in our step. 

The visibility `η(i,j) = 1/d(i,j)` is a heuristic that guides the construction of a route. This can change depending on the problem, here it is greedy, taking the closet node.

α and β are constants.

τ & η show the trade off between local and global knowledge. 

## Pheromone Laying and Evaporation

The amount of pheromone added by m ants is 

!["Pheromone Addition"](assets/pheromoneaddition.png)

`∆τ = Q/L_k` if kth ant used edge (i,j) in its tour, else 0. `Q` is a constant, `L_k` is the length of k's tour. This means that the ant will spread less the longer it has been travelling, emphesising shorter routes.

A trail evaporates with rate `ρ`, meaning our total pheromone after a time step is 

!["Pheromone Removal"](assets/pheromoneremoval.png)

## Parameters

The main parameters that can be tuned in ACO are:

* `N` the number of ants (N = 10...20)
* Evaporation constant `ρ` in pheromone update rules (typcially 0.99)
* The Power of `alpha` in the probability rule

The following occur in variants of ants systems:

* The min and max pheromone levels `τ_max` and `τ_min`.
* The probability of random moves `ε`

The following are **not** tunable

* The amount of pheromone between points
* The initial pheromone level

## ACO Algorithm

1. Initialise: Set pheromone strength to a small value
2. Transitions chose to trade off visibility and trail intensity; should we follow the crowd or do what we think is best? 
3. Each round the ants complete their route independent of the others, once they are all complete they update the pheromone

!["compare 1"](assets/compare1.png)

## Designing an ACO solution

* Problem representation
* Desirability heuristic
* Constraints 
* Pheromone update rule: quality of the solution: permit only feasible/valid solutions
* Probability rule: function of desirability vs pheromone 
## Variants

* We can weight the "best" ant's influence heavier than the others to have an influence from a global best

* We can use a min-max variant which only uses the best path to update the pheromones, this can help remove the noise from other ants.  We could also take the top N paths to help keep diversity in the solution space. 

### ACS

The ACS variant uses randomness to include exploration and exploitation. It adds:

* A random chance of choosing a city, the rest of the time the ants use the best (greedy) 
* Global updating rule, we use the best ant to update the pheromones
* Local updating, we change the amount of pheromone based on another heursitc e.g.  distance/path weight. 

### CACS

This version is designed for solving continuous problems, we distribute pheromone around the solution rather than at a set point. 


## Applications

* Bus routes, garbage collection (any varient on travelling salesman)
* Machine scheduling
* Network Optimisation
* Staff Placement

### Bin Packing

Given several items with different sizes and containers of a fixed size, how can we optimally pack the containers? 

We can first find a lower bound through intuition, we then use ACO to find the optimal packing.

Ants start with the empty bins, and add item by item. For example, take item `i` and move to bin `j`. We leave a pheromone trail on the series of placements.

We could use a local heuristic (how to choose the next node) to choose the largest possible container.

For this problem, we may want ot just use the best ant as we don't want the bad packs influencing this. We could also swap global and iteration best to help keep diversity.

To help escape local optima, we could allow ants to remove items. 

ACO is good for hybrid algorithms; it generally doesn't perform well by itself. 

## ACO Pros and Cons

* ACO is prone to premature convergence, this can be combatted by adjusting the evaportation rate/

* ACO can merge solutions to find a better one, however this can be a weakness in some problems.

* For real world applications, more complex variants such as ACS and CACS is needed. 