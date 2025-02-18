# Genetic Programming

 In computer programming, an **S-expression** (or symbolic expression, abbreviated as sexpr or sexp) is an expression in a like-named notation for nested list (tree-structured) data. S-expressions were invented for and popularized by the programming language LISP, which uses them for source code as well as data. 

![[Pasted image 20241118163716.png]]

Genetic programming (Koza) works well with LISP language, see slides for examples. However can we apply GP on higher level programming language? Yes, it's possible but it is really weird.
So GP can't really handle high level language. GP lost popularity due to higher level language and the abandoning of LISP. A new GP approach emerged: **Turing complete GP**.

The current usage of **Genetic Programming** is in Research and Machine Learning-like tasks.

### Features of GP

At the beginning the population is usually composed of random trees, with an initial depth is set. If we don't know what to do we could use different methods to initialize the population.
In the original GP the **parent selection is fitness proportionate**. Survivor selection follows a generational scheme, however steady-state is become popular for its elitism. (we are dealing with huge offspring).
In GP we set a certain probability to choose mutation or recombination: in 92, Koza used $p_{m}=0$, this means that we are not creating new genetic material (we are only recombining genes). In 92 instead Banzhaf set this probability to a low value $p_{m}=0.05$. In general GP is associated with a very low mutation probability.

Working with trees, the recombination approach is basically **exchange sub-trees among the parents**. In this way it's hard to maintain useful traits (desruptive crossover, so we need huge offspring). 

**Mutations**:
- Sub tree mutation: this could lead to big changes
- Node mutation: we change the value inside a single node
- Permutation: we change the order of operands
- Hoist: we select a sub-tree and we discard everything else. The sub tree is now used as current individual
- Expansion: the opposite of hoist, start from a node we expand it with some other nodes.
- Collapse sub-tree

In GP we have **Bloating** problem: an individual could be grow in size while maintaining a stable fitness. We need some mechanism to avoid this behavior:
- Prohibit operators that generate "too big" children
- Parsimony pressure: penalty for being oversize
- Fitness hole: we use tournament selection and sometime we let the smallest win

**Fitness**:
- Historically: % of success
- Perfectly suited for fitness proportional roulette wheel
- (for Symbolic regression): Absolute different from target, or mean squared error (non-linear is better)

GP vs. ES/EP/DE/PSO, all of them are dealing with floating point number.
GP has a non-linear structure, and a variable number of loci.

GP vs GA: we can see GP as a GA where the genome is a tree.

# Modern GP

Representation is a tree or a graph
The recombination is exchange of elements
Mutation: random changes
Population model: any
Parent selection: any
Survivor selection: deterministic

### Cartesian Genetic Programming

CGP represents programs as graphs in which there are non-coding genes.
Given some nodes, the idea is to tweak the connections.
Used a lot in evolving circuits. Main application in evolving arithmetic functions.

### Linear Genetic Programming

A variant of GP that evolves a computer programs as sequences of instructions of an imperative programming language.
More constrained and structured, it supports conditional branches.

### Grammatical Evolution

GE evolves which production production rules that are used in a Backus-Naur form grammar definition.
Unable to solve real-world problems, due to the non valid locality principle.
