# DE 

DE gets it name from the method of mutation where it takes the difference between individuals. 

DE represents individuals using a vector of real numbers in N dimensions, our fitness function evaluates the fitness at this vector. 

DE is unique as there is no noise source; the diversity is obtained from the mutations based of differences. 

## Method 

DE is a bit funky with the mutation and cross over steps.

**Selection** is done through a fitness function; bog standard. 

Our variance step is applied to each vector in turn; it does a weird combination of mutation and crossover:

1. Calculate a _differential_ individual by randomly selecting three from our population and taking the difference `a - F(b-c)`, F is a constant between 0 and 1
2. We then cross over this mutation with our individual using an independent trail for each dimesnion, if `p_c` is met, then we cross over this dimension. 
3. We compare the old individual to the mutated and crossed over one, if the new one is better we keep it. 

This process repeats for each individual in our population. 

## Setting F

`F` state our amplification factor, this tells us how aggressively to modify our solutions. This directly impacts our variance: if `F` set high then we will have a great amount of variance if `F` is low our mutated solutions will not differ from our original. 

We can find the variance of our solutions through this formula
![Variance Formula](de2.png)

`N` is the number of vectors 
`p` is the crossover probability

We can then calculate the factors of this:

![Variance Formula](de3.png)

This keeps the variance constant

## DE Variations

We can change our mutation strategy to use multiple differences e.g. `a+ F(b - c + d - e)`. Or we can compare against the best individual `x_best + F(b - c)

We can use a two-point-x-ver rather than a binomial one. 


