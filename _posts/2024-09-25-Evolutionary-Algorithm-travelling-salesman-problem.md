---
title: Evolutionary Algorithm (EA) - travelling salesman problem (TSP)
description: An app to visualize the evolutionary algorithm to solve travelling salesman problem (TSP)
author: chulmin
date: 2024-09-25 00:00:00 -0500
categories: [Artificial intelligence, Evolutionary Algorithm]
tags: [travelling salesman problem, TSP, Evolutionary Algorithm, Genetic Algorithm, EA, GA]
math: true
---

## Evolutionary algorithm
Evolutionary algorithms (EA) are a subset of optimization algorithms inspired by the process of natural selection, the mechanism behind biological evolution. EAs operate on a population of candidate solutions, which evolve through generations to solve a problem.

One of the most popular types of evolutionary algorithms is the genetic algorithm (GA), which mimics the process of genetic evolution. GAs represent possible solutions as chromosomes, apply operations like crossover and mutation, and select the fittest individuals to create the next generation of solutions.

### Terminology
- chromosome: A representation of a possible solution to a problem. For example, in the travelling salesman problem (TSP), a chromosome can represent a possible route.

<p align = "center">
<iframe src="https://drive.google.com/file/d/1SJrlMKfq5ezxUslCxpfqL2qJBvVeQKHe/preview" width="366" height="408" allow="autoplay"></iframe>
  <br>
  <em>Figure 1. Schematic of chromosome  </em>
</p> 

- crossover: A process where two parent chromosomes combine to produce one or more offspring. The idea is to combine good features from both parents in the hope that the offspring will inherit the best characteristics.
- mutation: A small random change applied to a chromosome. This ensures diversity in the population and prevents premature convergence to suboptimal solutions.

## TSP 

<p align = "center">
<iframe src="https://drive.google.com/file/d/1SOyrUKj8F-Rf3Jz6AmbFSDN5b8N5FFaZ/preview" width="553" height="395" allow="autoplay"></iframe>
  <br>
  <em>Figure 2. A set of cities  </em>
</p> 

The travelling salesman problem (TSP) is a classic optimization problem where the objective is to find the shortest possible route that visits a given set of cities and returns to the starting point. It is an NP-hard problem, meaning that finding the exact solution becomes computationally expensive as the number of cities increases. Evolutionary algorithms, particularly genetic algorithms, provide a heuristic approach to solving the TSP by evolving increasingly better routes over time.

### Genetic algorighm in TSP
Genetic algorithms can be applied to the TSP by encoding the routes as chromosomes and using crossover and mutation to evolve better solutions. Below are two critical genetic operations—order crossover and mutation—and how they are implemented in the TSP.

- Order crossover 

Crossover takes two parent routes and generates a child route by combining segments from both parents.
1. Segment Selection: A segment of the chromosome (route) from parent 1 between the indices start and end is directly copied into the child.
2. Filling the Remaining Route: The rest of the route is filled by iterating through parent 2 and inserting the cities that are not already present in the child. This ensures that the child represents a valid TSP route without duplicates.


- Inversion Mutation

Mutation is essential in evolutionary algorithms to maintain diversity and avoid premature convergence. In TSP, inversion mutation is commonly used, where a segment of the route is reversed.
1. Segment Reversal: The algorithm selects a segment of the chromosome between indices start and end. If start is greater than end, the indices are swapped to ensure the segment is valid.
2. Inversion: The selected segment of the chromosome is reversed, which corresponds to reversing the order of cities visited in that part of the route.
This mutation operator allows the algorithm to explore different route configurations without making drastic changes.

- Elitism

Elitism is a technique used in evolutionary algorithms to ensure that the best individuals (chromosomes) from the current generation are directly preserved in the next generation. This guarantees that the best solutions, which represent high-quality routes in the context of TSP, are not lost due to the random nature of crossover or mutation. By retaining these top-performing solutions, elitism helps the algorithm maintain a baseline of quality while allowing the rest of the population to explore new possibilities.

- New Chromosome

To maintain genetic diversity within the population, some of the chromosomes in each generation are generated randomly. This prevents the population from converging prematurely to a local optimum and introduces fresh genetic material, which can help the algorithm escape stagnation. The introduction of new, randomly generated chromosomes ensures that the evolutionary process continues to explore a broad range of possible solutions.

## Execution example

{% include embed/youtube.html id='3l-Ny6-VVAk' %}

### Circle ( Home page )
TSP with cities positioned on a circle is a common benchmark problem because it is a well-defined convex optimization problem with an analytical solution. The optimal route for cities arranged in a circle is simply the circumference, calculated as $2\pi R$, where $R$ is the radius of the circle.

This benchmark problem was selected to verify the implementation of the genetic algorithm in solving the TSP. To ensure robust convergence, the population size is selected as a power of the number of cities. Chromosome generation is divided into the following proportions:

30% are created via crossover,
30% are created via mutation,
20% are preserved through elitism,
20% are randomly generated to maintain diversity.

### Customized problem
In the customized problem, users can generate a random number of cities by pressing the start button. Unlike the benchmark problem, the cities are placed on a Cartesian coordinate grid, with a layout like a 4x4 grid. This creates a more challenging optimization problem, as it introduces multiple possible solutions and less obvious convexity.

By analyzing the performance of the algorithm on this problem, we can observe how it navigates the solution space when there are many valid routes. This variation offers insights into the algorithm’s ability to solve less predictable and more complex configurations of the TSP.

Chromosome generation takes the same proportions as previously.

<p align = "center">
<iframe src="https://drive.google.com/file/d/1Rt5kK_AsFEjV1-8jYa1tUahmfm7EJ0dH/preview" width="601" height="466" allow="autoplay"></iframe>
  <br>
  <em>Figure 2. Overview of the problem </em>
</p> 

- Analysis
<p align = "center">
<iframe src="https://drive.google.com/file/d/1RvPnzrg2BXab1UqqkU7D19HbnTMQYWfi/preview" width="601" height="466" allow="autoplay"></iframe>
<iframe src="https://drive.google.com/file/d/1RxjIpN0jGr4vcaBysjFYzSuZaxrJW5nv/preview" width="601" height="466" allow="autoplay"></iframe>
  <br>
  <em>Figure 3. Analysis </em>
</p> 
When the evolutionary algorithm first starts, all four types of chromosomes (elitism, crossover, mutation, and newly generated) lack any understanding of what the optimal route is. However, by the 91st generation, the chromosomes preserved through elitism begin to demonstrate a clear pattern, gradually identifying the shortest path. Chromosomes generated through crossover and mutation provide small, beneficial variations, helping to fine-tune the solutions that elitism preserves.

In contrast, the newly generated chromosomes—being randomly created each generation—continue to struggle, having no concept of the optimal route, while the other types make steady progress. This dynamic shows how elitism, crossover, and mutation each play a crucial role in refining the population towards the optimal solution, whereas the new chromosomes introduce exploration but lag behind in performance.


- result
<p align = "center">
<iframe src="https://drive.google.com/file/d/1S4Wa8BEfImlc_laEv0p-OD9Ap_2nl636/preview" width="601" height="466" allow="autoplay"></iframe>
  <br>
  <em>Figure 4. Result - Solid dots represent the maximum fitness, while empty dots indicate the minimum fitness at each generation. </em>
</p> 

In evolutionary algorithms, fitness is a measure that needs to be maximized, representing the quality of a solution. This contrasts with the concept of loss in artificial neural networks, which is minimized.

As shown in the result, chromosomes from elitism consistently occupy the top rankings across most generations, highlighting their contribution to maintaining high-quality solutions. Chromosomes generated via crossover and mutation also exhibit strong fitness values. Interestingly, the minimum fitness of crossover tends to be higher than the minimum fitness of mutation, suggesting that crossover maintains a more reliable baseline. Additionally, crossover shows less variation in maximum fitness compared to mutation, indicating that crossover provides more stability, while mutation allows for a wider range of exploration.


## Reference
- [TSP-Visualizer](https://github.com/Chulminator/EvolutionaryAlgorithm-TSP-Visualizer)
- [Chromosome picture source](https://medium.com/@ejamil_62933/today-in-medical-history-8-august-f8e7a1375ece) 


