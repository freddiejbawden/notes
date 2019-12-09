# Theory of Genetic Algorithms - Unit 10

_Count how many  times I misspell theorem!_ 

## Schema Theorem

### Schemata

We can represent a solution to a problem using a set of values and wildcards (*). For example a binary strign of length 6; with 1's at position 1,3,6 and a 0 at position 4 could be shown as `1*10*1`. 

We can define the order of the schema `o(H)` is defined as the number of fixed positions in the schemata. We also define the defining length `δ(H)` to be the distance between the first and last specific positions, in `1*10*1` we have `o(H) = 4` and `δ(H)` as there are 4 fixed positions and the distance between the first and last fixed position is 5; position 1 -> 6.

We can define the fitness of this schemata as the average fitness of all strings matching the schema. In `1*10*1` this would be the fitness of `101001`,`101011`,`111001` and `111011`. 

## Theorem

Holland's Schema Theorem states that for all short, low-order schemata with above-average fitness increase exponentially in successive generations. This is the foundation of GAs, as solutions with the best fitness will gradually take over the population until convergence occurs. 

Formally:

![](assets/holland.svg)

Where: 
* `m(H,t)` is the number of strings belonging to schema `H` in generation `t`. 
* `f(H)` is the observed average fitness of `H`
* `a_t` is the observed average for all solutions at generation `t`. 
* `p` is the probability that a crossover or mutation will destroy the schema `H`

The probability `p` can be expressed as; 

![](assets/holland2.svg)

Where:
* `l` is the length of the code
* `p_m` is the probability of mutation
* `p_c` is the probability of crossover

This means that a schema with a shorter defining length `δ(H)` is less likely to be disrupted. 

Holland's theorem is an inequaility rather than an equality due to the small probability that a string in H can be created from scratch by mutation.


### Issues With Hollands Schema Therom

The theorem only holds when the GA has an infinite population, meaning that this does not carry over to finite realistic systems. This can be due to the sampling error in the original propulation; a schemata may be drawn from a poor sample and thus won't have an advantage over another solution. 

Schema Theorem cannot explain the power of GAs it holds for all problems is that the inequality holds for good and bad solutions. The theorem doesn't state that the solution will be good for all problems; just that the best solution will take over no matter what; even if the solution was born from a bad sample and is actually poor. 

## The Building Block Hypothesis

The BBH poses that the reason why GAs frequently succeed in generating solutions of a high fitness. It states that GAs work by combining low order, low defining-length schemata together to create better and better strings from partial solutions. 

By reducing our problem into a set of paritial solutions, we dramatically reduce the complexity of the problem  allowing our GAs to perform. 

There is a lack of consensus regarding if the BBH holds. 

### Arguments Against BBH

* Collateral convergence: Once the population begins tot converge, measuring the average fitness of a schemata is irrellevant as we cannot estimate it using the current population. 

* Fitness variance: In reality, the fitness of a schema is arbitrarily far from the static average, even in the initial population.

* Compositionality: The superposition of fit schemata does not guarantess larger schemata that are more fit and that they are less likely to survive. 
