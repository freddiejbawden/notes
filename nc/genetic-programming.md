# Unit 6 - Genetic Programming

## Overview

Genetic programming takes inspiration from the structural nature of genomes to create a phenotype. We can provide a syntatctiic rules for programs can help restricting the complexity of a large search space. 

A classical example of GP is designing circuits, we have a series of inputs and outputs along with a set of rules to form the circuit, GP can use these rules to find a solution.

## How does it work?

A GP finds a  computer program to perform a user-defined task. Similar to GA but in GPs each individual is a program represented by a tree.

The search space is not exhaustible; as trees are potentially infinite in depth we can never test all solutions like in GAs.

### Evolving Programs 

We start with a population `P(0)`, for each program we run it on some input and see what it does. 

We breed fitter members of `P(0)` to produce `P(1)`.

### How do we avoid an error?

We use closure to help mitigate the
risk of dividing by zero for example by returning FLT_MAX. 

Sufficiency, we define a set of nonterminals need to be sufficiently large, terminals need to be defined. 

### Fitness

We run out program on a typical input data set for which we know the output. One issue is that we cannot test all cases so GPs can have fatal flaws if not tested correctly. 

We take the normalised value of the variance between the expected and actual output 

### Crossover

Crossing over is tough as itt could break syntactic rules. We must only cross over values that maintain rules

### Mutation

Mutation is also difficult as it can have dramatic effects on the outcome. We can shrink ( replace a subtree with a terminal) or cut a subtree. 

## Seeding

Assume one or more good programs exists for a problem, we can use these solutions as a starting point to help find different solutions 

## Using Defined Functions

We can use common elements such as loops, functions or conditionals to reduce variability due to commonly known structures.

## The Algorithm

1. Choose a set of functions and terminals for the program
2. Generate a n initial population of maximum depth d
3. Calculate the fitness of each program in the population use the chosen fitness tests. 
4. Apply selection, subtree crossover to form a new population. 

## General Points

* Sufficiency of the representation; we must choose an appropriate choice of non-terminals
* Variables: Terminal implied by the problem
* Program structure: Terminals also for auxiliary variables or pointers to functions
* We have a sufficient set of runs
* Local search: Terminals can often found by hill-climbing 

## Hybrid GP

We can use co-evolution to define fitness based on others in the population.

We can use inteligent editing, multiplying by 0 are not going to yield good results generally so we can kill these immeidatly.

