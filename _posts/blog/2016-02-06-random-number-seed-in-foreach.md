---
layout: post
title: "Random number seeds may not be what you want when using foreach"
modified:
categories: blog
excerpt: "Be careful when using foreach to perform parallel computing, because the random number generator seed may not be what you expected."
tags: [R, foreach]
image:
  feature:
date: 2016-02-06T00:48:00-5:00
---

We often use foreach to run for loops in parallel. However, for people who cares about reproducible research, he/she may not be able to reproduce the results if the random number generator is used in foreach, either explicitly or implicitly.

The standard way for obtaining reproducible results from a program that uses random numbers is to set the random number seed to a fixed number before actually perform the real computation. Let's assume our computation is just simply output 5 random numbers drawing from a normal distribution. The code is as follows.

~~~ r
set.seed(1)  # Set random number seed to 1
r <- c()
for (i in 1:5) {
    r[i] <- rnorm(1)  # Get one random number in each iteration
}
~~~

Here is the output:

~~~ r 
> r
[1] -0.6264538  0.1836433 -0.8356286  1.5952808  0.3295078
~~~ 

Now let's see what happens when using foreach and no parallel computing is used. The code is shown in the following.

~~~ r 
library(foreach)
set.seed(1)
r <- foreach(i = 1:5, .combine = "c") %do% {
    rnorm(1)
}
~~~

Here is the output:

~~~ r
> r
[1] -0.6264538  0.1836433 -0.8356286  1.5952808  0.3295078
~~~

It is exactly the same as the output from the first piece of code that use the plain for loop.

Finally, we try the third way, i.e., using random numbers in a parallel foreach loop. Here we use doMC package to register parallel backend. The code is shown in the following.

~~~ r 
library(foreach)
library(doMC)

registerDoMC(2)  # Register a parallel backend with two workers
set.seed(1)
r <- foreach(i = 1:5, .combine = "c") %dopar% {
    rnorm(1)
}
~~~ 

Here is the output:

~~~ r
> r
[1] -0.1624277  0.4536430  0.3282893 -0.7025522  0.6008962
~~~ 

If re-run the code again and print the output, the results will be different. Here is an example output after re-run the whole code including "set.seed(1)" to set the random seed.

~~~ r
> r
[1] -0.77484781  0.09627244 -0.79425302  1.08190738  0.24735862
~~~

As we can see here, the outputs are quite different when we re-run the code. The reason is that a parallel foreach will fork several new processes (the number depends on how many parallel workers are registered in the parallel backend). Each process will have a new random seed by default. And the random seeds are not the same as the one we set using set.seed() in the main process, which is a different process from the processes created for the foreach statement.

Based on these observations, we need to be careful with parallel foreach if we expect reproducible results. A potential solution to generate reproducible results when using parallel foreach is that we should set random seeds within the loop. For example, we may use code similar to this one.

~~~ r 
library(foreach)
library(doMC)

registerDoMC(2)  # Register a parallel backend with two workers
r <- foreach(i = 1:5, .combine = "c") %dopar% {
    set.seed(i)
    rnorm(1)  # For practical use, we should get many random numbers in each loop.
}
~~~ 

This piece of code should generate the same result every time we execute it.

To summarise, we should set random number seeds within the foreach, if a parallel backend is registered AND %dopar% is used to instruct foreach to use the parallel backend AND we use random numbers in the foreach loop AND we want the results to be reproducible.

Note: Random numbers could be implicitly used even if we do not see random number generation calls such as rnorm(). An example is to use the k-nearest neighbor clustering algorithm within foreach().

