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

## Basic Choices

When creating a GP system, there are 5 things we must consider; 
* Terminals
* Functions
* Fitness function
* Control parameters
* Termination criteria

Terminals and functions are components of the programs, e.g. variables or operators. 


## Syntax Types

There are many ways of representing a GP

### Pedestrain GP
Uses a traditional GA binary string representation. Each bit string was made up of 225 bits which were a series of 5 bit words. These were encoded into a set of functions and terminals.

Bit level mutation and crossover genetic operators were used to generate new individuals. This has previously been used to create a simple integer sequence

### Strongly Typed GP

Pedestrain GPs do not take into account whether the program makes sense to run, e.g. divide by zero errors. This may reduce exploration as we are cutting off branches sooner. 

### Minimum Description Length

MDL bases fitness not only off the fitness but its size. We can use MDL in the form;

`MDL = Tree_Coding_Length + Exception_Coding_Length`

`Tree_Coding_Length = (n_t + n_f) + n_t*lgT + n_f*lgF` where, n_t is the total number of terminals and n_f is the total number fo function calls in teh tree. T is the number of terminals in the terminal set, and similarly F is the size of the function set.

`Exception_Coding_Length = x∈Terminals: ∑L(n_x,w_x,n_x)` where n+x is the number of cases represented by the particular leaf, x and w_X is the number of cases which are wrongly classified. L(n,k,b) is the total number of bits required to encode n bits given k 

### Stack Based GP

We can use a prefix structured tree sim. to LISP to generate programs. 

### Machine Code GP

We can use raw machine code, this is good for optimising but blows up the search space. 

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

We can use intelligent editing, multiplying by 0 are not going to yield good results generally so we can kill these immediately.

## Troubleshooting

GPs can be tricky due to the large search space. To help you succeed with them, you can:

* Study your population; means, varaiance, depth, size etc.
* Checkpoint your results
* Control bloat by punishing large programs
* Accepting that no program is error-free. 

## Cartesian Genetic Programming

Rather than have a program represented as a tree, we use a graph built from primitives. This allows for more complex evolution of the GP. 

We use a two dimensional method to allow the network add more complexity by passing values between nodes no just processing it in one.

Many nodes don't effect the output, this is similar to junk DNA. To combat this we choose a small group of functions to hopefully evolve the useful ones. 

## Neuroevolution

Designing CNN architectures still requires expert knowledge and a lot of trail and error. 

Neuroevolution uses genome for high-level building blocks of a neural network, such as 
* convolution
* max pooiling
* summation
* concatenation

The phenotype is the actual neural network, fitness is determined bt training on a test set




