# Genetic Algorithms

Genetic Algorithms takes the evolutionary metaphor a step further by trying to simulate genetic pools and breeding. 

## Problem Solving Using GAs

1. Find a representation of solutions e.g. S = (length of legs, angle limits, motor power)
2. Define an objective/fitness function to be maximised that can be calculated for each S
3. Generate a population of candidates (bag of S)
4. Repeat the following until termination criterion (max iterations or threshold met)
    * Evaluate candidates 
    * Choose high-fitness solutions 
    * Mutate some solutions

## Simple GAs
### Mutation in GAs

Mutations take place after all the solutions in the pool have been evaluated. There are two main types of mutation that take place.

#### Reproduction & Crossover

Once the poor solutions have been "killed" off, we must repopulate our gene pool. This is done by splicing existing solutions together. For example:

!["Crossover"](assets/crossover.png)

#### Mutation

We can randomly change values in our solution to keep our gene pool fresh; as otherwise we would tend towards one solution very quickly. 

Example:
`ABCDE -> ABCAE`

Mutations are usually performed after crossover with very low probability.

### Pros and Cons
**Pros**
* Small problems often can be solved optimally using this strategy. 

* Large problems however take much longer to reach a near optimal solution due to the amount of variance.

* It can be run quickly as gene pool evaluation and mutation can be parallelized. 

* Adding  new constraints is simple, as only the mutation and fitness functions need updating

**Cons**
* Representing complex problems in a data string can be tough.
* "Good" Fitness solutions are often tough to create
* Many parameters to optimise; population size, fitness cut off, mutation rate etc.
* Valid and useful mutation and crossovers can be difficult to form

## Canonical Genetic Algorithms