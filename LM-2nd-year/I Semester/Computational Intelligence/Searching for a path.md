 # Searching for a "path"

In this new context we are searching for a **path**: we can consider "a path", a **sequence of actions** to solve a problem. In each step we must decide what to do next.
- We find a path **from the initial state to the goal state** (in the solution space)

Recap of notation:
- The **problem space** is what we want to solve, to do
- The **state space** is an abstraction: it can describe the situation we are in, it contains all relevant information to solve the problem (Something we can omit something, and be satisfied to solve a simpler problem)
- The **solution space** is composed of states that makes sense, by restricting the state space.
We shift our attention to solving to searching.

We are also making some assumptions on the problem:
- The problem is **sequential**, i.e. it can be solved with a sequence of steps
- It's **observable**, i.e. all relevant details are known
- It's **deterministic**, i.e. the effects of actions are predictable
- It's **static**: i.e. time is not relevant for choices
- It's **Discrete** i.e. the alternative actions can be enumerated
- Only **one actor** is active, so my actions don't influence others' actions.
- Note that searching algorithm can be applied to infinite problems: it is sufficient that the actions at each step are enumerable

### Greedy best first

A simple approach to search for a path is **greedy**:  move forward to the next city trying to head in the direction of Bucarest (don't go back to previous city).
Sometimes this approach could get stuck and we need to ignore the rule to not move backward. Also this approach requires **extra knowledge** about the problem (in the **Bucarest example** is the direction towards Bucarest itself).

### Generate n test

If we don't have knowledge on the problem we could **move forward to the next city at random**: we just never go back to a previously visited city.
We can apply backtracking only if we get stuck.
However this is a perfectly reasonable strategy.

More formally, this is an **heuristic search technique** which **guarantees** to find a solution if done systematically (and of course, if a solution exists).
It is based on a **DFS with backtracking**. A good generator is exhaustive, not redundant (should not generate the same solution) and informable

### Breadth first

We can distinguish between:
- State completed
- State discovered but not completed
- State not discovered yet

Tree (graph) search: a tree is **superimposed** to the search space, there is no tree in the problem, but it's in the algorithm.

![[Pasted image 20241108182429.png]]

Tree search doesn't remember visited nodes, but it's exponentially slow, but memory efficient.
Graph search keeps a graph in memory: much faster but heavier. So this is a time-space trade-off.

### Generic search

State vs Node:
- A state is something that fully describe the problem.
- A node is the storage adaptation of a state
In the problem described above with BFS the nodes are on the left, while the states on the right.
A function $A(s)$ yields the set $\{a_{0}^s,a_{1}^s,a_{2}^s\dots\}$ of a possible actions that the agent can perform in state $s$.
We also need a function $R(s,a)$ that calculate the results of performing action $a$ in state $s$.
A function $C(P)$ yields the cost paid by the agent to cover the path $P$: it can be simple such as the sum of the cost of each step.

A **general code** is the following:

![[Pasted image 20241108184921.png]]

We call "**frontier**" the set of states discovered but not examined. We can implement the frontier as a priority queue. This above is a family of algorithm, just changing the priority queue we can create different algorithms.
The **goal test** checks if a node (state) satisfy the problem: the goal test is considered NOT during the discover phase, but only when the node is examined (i.e. exit the frontier)

### BFS

The root node is expanded, then its successors, then their successors...
It is a **systematic**, always **complete**(if branching factor is finite), and optimal when all costs are equal. So BFS cannot be applied to infinite branching problem.

### Dijkstra

Like BFS, but the node with the lowest path cost from the root is expanded. It is a non informed algorithm. BFS can be thought as a Dijkstra with uniform cost.

### DFS

Deepest node in the frontier is expanded. We are dealing with cycles problem. It consider the first possible solution. We can combine DFS with a bound, to reduce the number of operations. Another possibility is to limit the maximum depth. Iterative deepening is also used.

### Beam search

BFS has the problem of high branching. To avoid this problem i can simply cut nodes fro the frontier. We need to keep in the beam (frontier) the most promising solutions.
# Problem: Min sum

Given an array of $n$ positive integers, find the **minimum number of elements** that **sum to $G$**.
Let's think the **state as two sets**: one set with picked items and one with the left ones.
Using a simple depth first search requires less memory but does not guarantee the optimal solution. Breadth first requires more memory but it is able to achieve the global optima

# A* Algorithm

**Informed search algorithms**, also known as **heuristic search algorithms**, are an essential component of Artificial Intelligence (AI). These algorithms use domain-specific knowledge to improve the efficiency of the search process, leading to faster and more optimal solutions compared to uninformed search methods.

A simple heuristic search is **greedy best first search**, where at each node we move to node with the lowest **heuristic**, however this approach is not optimal, it could get stuck into local optima and it's incomplete (unless backtracking).

An example of Informed search is the **A* algorithm**: complete and Optimally efficient.
It always computes the path with minimum cost by expanding a minimum number of nodes
Under **reasonable assumptions**
A* computes for every node a value, known as F value that need to be minimized.
$$
f(x) = g(x) + h(x)
$$
In the above formula, $g(x)$ is the **actual cost** while $h(x)$ is the **heuristic**, i.e. an estimated cost to reach the solution.

To **guarantee optimal solutions**:
- $h(x)$ **must be admissible**:  it never overestimates the cost to reach the destination node (in a minimization problem). If the search space is a tree, admissible heuristics guarantee optimal solutions
- $h(x)$ **must be consistent**: for every node $n$ and every successor node $ns$ of $n$ it must be satisfied $h(n) \leq \text{cost}(n,ns) + h(ns)$. In other words $h(x)$ is **monotonic**.
Note that:
$$
\text{consistent } h(x) \implies \text{admissible } h(x)
$$
In general, but it **not always true**, also the opposite is true.
How to Create Admissible Heuristics? Simplify the problem by relaxing constraints.
Time complexity of A*: $O(bd)$, where $b$ is the branching factor and $d$ is the depth of the optimal solution.
