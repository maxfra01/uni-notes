# Evolutionary algorithms

There is an obvious connection between a **learning process** and **evolution**.
An **Evolutionary algorithm** uses mechanisms vaguely inspired by biological evolution, such as **reproduction**, **mutation**, **recombination**, and **selection**. 
"EA are generic population-based meta-heuristic optimization algorithms vaguely inspired by biological evolution".
- Why population-based? In EA we have a population moving to other solutions, they can even interact with each other.
- Why optimization? EA are employed in many fields, and **also** for optimization.
- Why meta-heuristic? It means that we don't know what to do, so we try trial-and-error to optimize something.

Accumulation of small changes can lead us to a new solution: 
- **Variation processes are random**
- **Selection** is mostly **deterministic**.

```ad-note
title: Connection with Hill Climbing
This concept is similar in HIll Climbing: the tweak function is mostly random, while the selection can be deterministic (moving to a better solution if it has a higher fitnees). 
```

In EA we don't need to design changes, but we need to select carefully the mutations.
Pay attention: Evolution **is not a random process**; randomness gives us the "material" to build a solution, but our choices matter.
Also, Evolution is not an optimization process, does not have a goal, does not favor strength nor intelligence.
However, when all variations are accumulated in a specific direction, the final product may look like the product of an intelligent design. (That's the impressive part of evolution!)

In practice, we'll use the **fitness function** to push our variations in a certain direction.

# Evolution strategies

- The challenge in ES is selecting the right **mutation step (s)**, as it influences the success of the strategy.
- Methods to determine $s$ include:
	- **Random Guess**: Simply selecting an arbitrary value for $s$.
    - **Experience**: Using prior knowledge or intuition to set $s$.
    - **Meta-Optimization**: Automatically optimizing $s$ based on the problem's characteristics.
- In ES, mutation step $s$ can be dynamically adjusted during the search by monitoring the number of successful mutations over an **era** (a set number of iterations).
- **Tweak s**: Depending on how many successful mutations occurred, adjust $s$ to balance exploration (large mutations) and exploitation (small mutations).
- The key is to adapt $s$ **endogenously** based on the performance of the algorithm, ensuring the search behaves appropriately for the current problem context.

### (1+1)-ES 

- **(1+1)-ES** is a first-improvement hill climber where a single solution is mutated to generate one new candidate solution per iteration.
- **Gaussian Mutation** is used to modify each element of the solution. The new solution's element is calculated as: $$xi′​=xi​+N(0,s)$$ where $s$ is the **mutation step**, controlling the variance.
- The goal is to find a balance between **exploration** (searching broader areas) and **exploitation** (focusing on improving the current solution).

### (1+λ)-ES

- In **(1+λ)-ES**, multiple candidate solutions (denoted by $\lambda$) are generated from the current one through Gaussian mutation.
- This is a **steepest-step hill climber**, where the best candidate from the $\lambda$ new solutions is selected to replace the current solution if it improves the fitness.
- This method increases the search scope, allowing multiple mutations per iteration to explore more potential solutions.
- It uses a **plus strategy**, meaning the new candidate is selected from both the current and $\lambda$ mutated solutions, which encourages a thorough exploration.

### (1,λ)-ES

- Similar to **(1+λ)-ES**, but instead of comparing the best candidate with the current solution, **(1,λ)-ES** always replaces the current solution with the best candidate generated from the $\lambda$ mutations.
- This is known as the **comma strategy**, where the current solution is not retained, even if the new solutions are worse.
- This forces continuous exploration, especially with large $\lambda$, which helps escape local optima at the cost of potential short-term performance dips.

### Strong and Weak Causality

- **Strong Causality** refers to systems where small changes in the solution cause proportional changes in fitness. It ensures smoothness in the fitness landscape, making it easier for local search methods to work effectively.
- **Weak Causality**, on the other hand, refers to situations where changes in the solution lead to unpredictable, discontinuous shifts in fitness, making optimization harder.

### (μ,λ)-ES and (μ+λ)-ES

- These strategies extend **(1+λ)-ES** by considering multiple parent solutions $\mu$ rather than just one.
- In **(μ,λ)-ES**, $\lambda$ offspring solutions are generated from $\mu$ parent solutions, but the parents are discarded after the new solutions are evaluated (comma strategy).
- In **(μ+λ)-ES**, the offspring are generated, and the best $\mu$ solutions (from both the parent and offspring) are retained (plus strategy).
- These strategies are among the first full-fledged **Evolutionary Algorithms** (EA), as they involve populations of solutions and mutation-driven evolution over time.

# Essential evolutionary algorithm

All environments have **finite resources**, that means that they can support only a limited number of individuals. Selection is inevitable, some individuals will die, others will survive.
In particular the ones that compete for the resources have increased chance of reproduction.

**Genotype**: internal representation of the problem, that genetic operator directly manipulates.

**Phenotypic traits**: They are traits that affect the fitness value. The useful traits are associated with high fitness. So individuals with useful phenotypic traits
- have higher chance of reproduction
- Portions of their genotypes will tend to increase in subsequent generations

A **Population** consists in a set of diverse individuals. **Variations** occur randomly (source of diversity). **Individuals** are "units of selection":, so we can select a single individual with a particular **gene**, that will be spread among the population.

In practice:
- A **Candidate solution** -> Individual
- A **Set of candidate solution** ->Population
- **Sequence of steps** -> Generations

To represent an individual we need to **encode** a potential solution for the problem.
1. Then we need to do **parent selection**: it just means that we need to select some parent individuals to run the next generation.
2. To mutate the population we only look at the **genome**, we mutate it
3. Then we recombine the new individuals in the population (**Offspring**). 
4. Then we can evaluate 
5. And also perform survival selection.

![[Pasted image 20241010165204.png]]

We need some randomness in selecting the parent. In the survival selection we only select the best individuals.

### Terminology

- The algorithm cultivates a population of individuals
- An individual is determined by its genotype and has a fitness value associated to it
- Genotypes contain genes, which are in (usually fixed) positions called **loci**
- **Alleles** are alternative genes that can occupy the same locus
- The genome is a list of genes
- The chromosome is the list of genes
- The chromosome is the genotype after mapping
- The fitness is the phenotype of the individual
- The allele is the value of a gene

### Novelty vs Quality

- Pushing towards **novelty** means increasing population diversity, through genetic operators.
- Pushing towards **quality** means decreasing population diversity, through an accurate selection of parents and survivors.

### Exploration vs exploitation

- Exploration in EA is associated with **recombination** of two or more solutions to create the offspring.
- Exploitation in EA is associated with **mutation**, i.e. a small change in an individual.

### Selective pressure

How much we are favor the champion individual.
A high selective pressure means that genes from champion are dominant.
The **takeover time** $\tau^*$ is the number of generations until the applications is filled with copy of the champion individual (i.e. parent selection is useless)

### Recombination

One possible strategy is **1-cut crossover**: take 1 randoms-size half of a parent and mix it with the second half oh other parent.
Doing recombination with this approach does not take traits in consideration.

Since we don't know traits, we need a technique that blindly preserves traits.
One possible strategy is, given A and B (parents), take randomly one gene from A and the one from B, and so on...

mutation: only one parent
recombination: more than one parent
### Population management models

- **Generational approach**: each offspring replaces parents, this is called $(\mu, \lambda)$, each generation has max age 1
- **Steady state approach**: $(\mu + \lambda)$ strategy, individuals do not age.
We can add **elitism** to this approach by making the $n$ fittest champions to not age.
### Parents selection

This phase can have randomness.
First strategy is **roulette wheel**: all individuals have a weight (depending on fitness). We can choose randomly but fitter individuals have higher probability to be extracted:
$$
p_{i} = \frac{w_{i}}{\sum_{j}w_{j}}
$$
A second strategy is **uniform selection**, where all individuals have the same weight. This is a low selective pressure approach.
$$
p_{i} = \frac{1}{\mu}
$$
A most common approach is known as **fitness proportional** (which is similar to roulette wheel): in this case the probability is directly proportional to fitness.
$$
p_{i} = \frac{f_{i}}{\sum_{j}f_{j}}
$$
This approach is problematic when we are dealing with "numeric fitness", for example 10004, 10003, 10001 fitness values have the same probability, so this won't work.
If the $\Delta f$ between two individuals is meaningless, then fitness proportional is broken.
There are **variants** that try to solve this problem, for example **windowing** that remove the worst fitness $\beta$ from the last $n$ generations: we calculate the fitness as $f'=f - \beta$.

Another technique derived from roulette wheel is called **rank-based**: let's imagine we have the following fitness values: 10005, 10004, 23. We can sort the fitness values, so we assign a rank, then we do classic roulette wheel with the rank (based on proportional weight). Of course there is some overhead due to sorting fitness, but usually it is negligible.
The approach described above is called linear ranking, we can also use **exponential** ranking (more selective pressure).
$$
w_{i} = r_{i}^p
$$
Note the if $p=0$ then we have uniform selection, while if $p\geq 1$ then we have linear or exponential ranking.
This concept is important because we can move through strategies by only tweaking a parameter $p$.

Another concept: **Tournament selection**.
We just pick $\tau$ members at random, then select the best out of these. If $\tau = 2$ then we have the same selective pressure as linear ranking. This tournament approach does not require ordering. This approach allows to select the individual with lowest fitness.
Again $\tau = 1$ is the uniform selection.
It's possible to have a floating point $\tau$ number, for example 3.9 means that we take 3 individuals and another one with 90% probability.
If $\tau = \infty$ then we are always selecting the champion.

Practical advise: use tournament selection.

### Survivor selection

This is a **deterministic phase** (usually).
Survivor selection can promote diversity: for example **crowding** let individuals live based on how much they are different from each other.

### Genetic operators

We would like to have a representation that works on every problem, but this is not always possibile (for example bits stream can be infeasible for same problem).
Operators **shape the fitness landscape** (they define who is similar to who)

Historically we had the following steps: 
1. Select parents
2. Mutate the parents with probability $p_{m}$
3. Perform crossover with probability $p_{c}$

The modern GA approach:
1. Select parents
2. Perform crossover
3. Mutate the offspring with probability $p_{m}$

Hyper modern approach:
1. Select a genetic operation with probability $p_{i}$
2. Select the correct number of parents (i.e. 1 for mutation)
3. Generate the offspring
Note that selecting an operator is like the multi-armed bandit problem.
# Representation

### Integer representation

We are working with a fixed-length integer array that can contains:
- Numeri variables
- Integer-coded values (for example xs, s , m, l, xl) such as **discrete values, categorical attributes and one-time items**.

Possible mutation:
- Random resetting: an allele in a random locus is replaced with a random value
- Creep mutation: the allele in a random locus is slightly increased or decreased
Note that not all mutations make sense in every context, for example a creep mutation in categorical attributes makes no sense.

Possible crossover:
- Recombination (same as usual)

### One time items

This integer representation is composed of unique elements. The traits are in the **absolute position**, in the **order** and in **edge/vicinity**.
Possible mutation are:
- **Swap**: minimize the changes in absolute positions.
- **Scramble**: Some alleles are randomly selected, then shuffled. Also here we are minimizing change in absolute position (pure local search idea)
- **Insert mutation**: Two loci are randomly selected and the allele in the second moved next to the one in the first, shifting along to make room ![[Pasted image 20241101145005.png]] This minimize the changes in the order 
- **Inversion mutation**: minimize change in edges (most neighbor stay neighbor): ![[Pasted image 20241101145117.png]] 

Possible crossover (most of the standard crossover are disruptive):
- **Order crossover OX**: trying to preserve the relative order
- **Cycle crossover**: ![[Pasted image 20241101145514.png]]
- **Partially map crossover**: ![[Pasted image 20241101145532.png]]
- **Inver Over crossover**: (useful for tsp lab) Select a gene from the first parent, locate that gene in the second and take the gene next to it. Then make the two gene close and fill with the remaining gene.
![[Pasted image 20241101145837.png]]
 

### Floating point representation

![[Pasted image 20241028114205.png]]

**Mutation probability** $p_{n}$ is the probability with each single locus is modified or not.
**Mutation strength**: perform another mutation if probability is lower than a certain value $\sigma$ 

Crossover: 
- Uniform crossover: take a part from a parent and the other and combine them. (most of the time it won't work, dealing with optimization or similar)
- **Local intermediate crossover**: the idea is to find an average between each locus of the two parents. It may vary on a parameter $\alpha$ that can be self-adapted or fixed.

# Evolution strategies

In $\mu + \lambda$ the takeover time is decided by the ratio between $\lambda$ and $\mu$, because with a high probability, the offspring is very similar to the parents.
$$
\tau^* = \frac{\ln \lambda}{\ln \frac{\lambda}{\mu}}
$$

# Evolutionary programming

Typically applied to: prediction by finite state machines.
The goal of Fogel was to evolve FSM to make predictions.
Nowadays EP is merger with EA.

# Genetic programming

Typically applied to ML tasks and program synthesis.
Main features: huge population (not good at creating new traits, only combining them), slow, non-linear chromosomes.

(Having a big offspring is useful when using disruptive operator)


# Learning Classifier Systems

Classifying system based on tuples with (Rule, Action), which is our representation

# Differential Evolution

Replace strategy for EA, often used in parameter optimization (floating point context)
Main features are:
- 4 parents to generate a single offspring
- Effective with non-linear and non-differentiable continuous function.
- It does not require gradient descent
- Uniform crossover
- Differential mutation
- Generational model 
- Uniform parent selection
- Survivor selection: parent vs offspring  (deterministic niching)

The **differential mutation** starts with a given candidate solution. A new offspring is generated by adding a perturbation vector $p$:
$$
x' = x + p
$$
Where $p=F \cdot (y - z)$ where $y,z$ are randomly chosen individuals and $F$ is just a scaling factor.

There are different variants of DE.

# PSO

Another replacement for EA used for numerical optimization.
No crossover is used, every solution carries its own perturbation vector, **so each solution has its own direction** regardless of the other individual. 
Mutation: tweaking velocity vector
Generational model, deterministic parent selection .

The perturbation vector can be define as the weighted sum of:
- the current velocity vector
- The difference from the personal best known positions
- The difference from the global best known position


# EA and ML

Both refers to the same starting paper "Machinery and Intelligence" from Turing.
The term "Machine learning" was popularized by Samuel in a paper describing an evolutionary approach for playing checkers.
Other papers cites "Machine Learning" as a keyword.

A common aspect of both mechanism is learning without the need of human expertise.
There are strategies that use evolutionary algorithm to tune hyper-parameters of a model.

# Biggest problem in EA

Premature convergence: the tendency of an algorithm to converge towards a point where it was **not supposed to converge**. That could be because of too fast algorithm.
John Holland defined the problem as **Lack of speciation**: EA general inability to exploit environmental niches (place of environment with limited resources).

Divergence of character: the more the coinhabitants of an area differ from each other, the less they will compete with each other, therefore natural selection will tend to favor any variation toward greater divergence.

Niches favor the divergence of character: how can we create a niche in EA?

Exploration vs exploitation: if the parents are very similar, then recombination (i.e. exploration) makes no sense. But what does it mean that parents are "similar"?

Niching: grouping similar individual, but how can we do it? Basically we can group individuals based on genotypes, but it would be better if we can group individuals based on the phenotype but it is not clear how to compute it.
Explicit neighborhood if we have some kind of measure of similarity
Implicit when i only think that they are similar (for example a son of an individual is probably similar).

Diversity can be seen as a distance metric: from a single individual or from a subset of population.
Diversity can be also seen as a property, but at which level?
- Fitness is simple, but often useless and trivial
- Phenotype (ad hoc)
- Genotype

One way to promote diversity is **altering the selection probability**.
The Lineage-based has a $\xi$ value that does not depend on the individual but on the environment (island model).
The Genotype based is particularly effective when it is possible to define a sensible distance between genotypes.
The Phenotype is usually impractical. Sometimes fitness could be used as a proxy for phenotype distance (in multi objective EA).

Let's call Parent selection $\alpha$ and Survivor selection $\omega$

### Island model

Population is partitioned into sub-populations: only local interactions are allowed.
Periodically, **migrants** are moved between island.

![[Pasted image 20241104124329.png]]
### Segregation

Population is partitioned into N sub population. Again only local interactions are allowed.
Instead of migrants we just merge together into N-1 population, to avoid stagnation.
Increasing population size is decreasing the selective pressure (slower approach)

![[Pasted image 20241104124341.png]]

### Hierarchical fair competition

The population is partitioned into sub-populations with similar fitness. Only local interactions are allowed.
- The offspring is promoted or demoted according to fitness

![[Pasted image 20241104124406.png]]


- Niching with implicit neighborhood (not actually measuring individuals, just looking at their fitness)
- Reduce competition between newborns and already optimized individuals (ladder)

### Cellular EA

We don't allow each individual to recombine with any other. Individuals can interact only with their neighbors. This interaction limitation takes down the takeover time and slows down the process.

### Deterministic crowding

Offspring compete against parents for survival.
Again niching with implicit neighborhood.

### Allopatric selection

The whole offspring compete for survival. Genetic operators that create large offspring can be exploited without the risk for the offspring to invade the population

### Fitness sharing

Multi objective problem are the only that make sense with fitness sharing

### Standard crowding

### Crowded comparison operator

### Lexicase selection

We have a certain number of goals, like solving different tasks.
Instead of prioritizing one task against one other, we shuffle the order of the task.
The effect is the increase in specialization, that could lead to diversity and optimal solution

### Restricted tournament selection

### Gender

We add a gender to individual  and enforce sexual reproduction.

### Random immigrants

Periodically insert random individuals in the population: could be useless if we insert random individuals. Instead it could work if we are promoting novelty.

### Extinction

Upon convergence remove a significant part of the population then fill it with some randoms and the champions. However that could lead to complete takeover 

### Add diversity as a goal of the problem

