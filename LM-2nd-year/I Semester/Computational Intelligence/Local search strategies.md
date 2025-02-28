# Heuristic classification

Let's see some basic concept:
- **NP problem**: Non deterministic polynomial time problems, can be solved with a non-deterministic touring machine. In other words we can check the solution in polynomial time, but not generating it.

- **Black box algorithm**: whenever you decide to ignore the internal details of an algorithm, constraints may not be explicit, black box has no memory.

# **Local search algorithm**

Local search algorithm is an **heuristic method** for solving computational **optimization** problems.
Target problems are the ones that can be formulated as a maximization (or minimization) of something.

The idea is to find a technique that starts with a solution and find a better one. The algorithm just **analyze the surrounding solutions around the current one** and eventually moving to a better one. An example of this is **Gradient descent**.
In local search we are only interested in solutions, so we can think of it just like a black box algorithm.
1. Generate a starting solution (random, heuristic...)
2. Creare neighbor solution by adjusting something from the current solution
3. Replace the current solution with the new one if it's better
4. Re-do step 2
Example of local search algorithm can be: Gradient Descent, **Hill Climbing**, Simulated Annealing, Tabu Search...

We are looking either for a viable solution (with constraints) or the best solution. In general we are looking for a "**reasonable**" solution. Maybe we eventually run **out of time**.

Example: **8 queens** can be cast as a black box pure constraint satisfaction algorithm, but can also be cast as a minimization of the queens threats.

Example: **University timetable**: big search space, a "good" timetable has several criteria. we also need that timetable is feasible

### Hill Climbing

Hill Climbing is a **mathematical method** belonging to local search algorithms.
The idea behind this method is to tweak (increment) a solution to generate a new candidate solution, and eventually move to this new solution if it's better according to some metrics. This technique quickly climb up to a local maximum. There are no guarantee that the solution is a global optimum.
There are 2 main improvement of hill climbing:
1. **Random Mutation Hill Climber** (RMHC): we choose different starting points, then choose the best.
3. **Steepest-step Hill Climbing** (Steepest Ascent HC): starting from a point we explore a finite number of steps in a direction and we move **only if** the final position is better than the starting one.

The stopping conditions can be:
- Best solution found
- Total number of evaluations
- Total number of steps
- Wall-clock time: required in practical situation
- Steady state: give up if change of improvement is low

Example: **1 Max problem**

The goal of this problem is to build a string of ones (or an array of '1', True...) starting from a random string.
The "**fitness**" is the sum of the ones: this tasks is easy because it's unimodal, separable, non deceptive.

A possible variant is **2 max**: the goal is to have all ones or all zeros: the machine needs to take a decision, then optimizing it.

There are also **Noisy 1 max**, Dynamic 1 max, weighted 1 max, multi-objective 1 max...

# Fitness landscape

In computer science, **Fitness** is the ability to solve a given problem. Often we need to minimize or maximize possible solutions, so a key phase is how to choose a fitness $f(x)$ to minimize evaluation $E(x)$
Sometime we can switch between minimization and maximization, but it is not always possible.
A **fitness function** is a particular type of objective function that is used to summarise how close a given solution is to achieving its goals.

A **fitness landscape is a way to represent our search space**, where the "hills" are our solutions. We are able to evaluate a certain point in the space with a fitness function.
Highest peak represents the **global optima**, while the lower peaks are **local** optima: we can say that solutions are attracted by optima.

![[Pasted image 20240930120517.png]]

Some terminology about problem:
- **Unimodal** means there's only one peak.
- **Multimodal** means more than one local optima.
- **Isolate fitness landscape**: landscapes with a relatively small number of optima
- A **valley** is a region of low fitness, often separated by **plateaus** (mesas, aka vast areas of uniformly high fitness, not so good, unable to navigate) or **ridges** (elevated areas that connects peaks, can be seen as a sequence of local optima; ridges provide a pathway to navigate the landscape, but they can be bottlenecks)
- A **Funnel** is a region where the fitness increases rapidly towards a particular point (can be a bottleneck)

### Well known landscape

**Needle in a haystack**: vast region of low fitness, and only one point of high fitness, but there are no funnel.

![[Pasted image 20240930121306.png]]

**Rocky**: noisy, hilly, and no funnel

![[Pasted image 20240930121345.png]]

**Deception**: a landscape with a fitness function that intentionally misleads the optimization process. Local optima are more attractive than the global one.
### Exploration vs exploitation

This dilemma is a fundamental concept in decision making.
- **Exploration**: seeking out new information, delving into unknown to discover something new
- **Exploitation**: utilizing current knowledge to its fullest potential, Choosing the best-known option based on existing information to maximize immediate benefits.
For example in 1-max problem, doing small steps is a exploitation approach, while larger steps is exploration approach.

Known problem: Multi-armed bandit problem, multiple levers to pull, goal is to find the highest-paying machine.

### Simulated annealing

We are in a situation of a **steady state**: we need to escape that state.
The idea of **simulated annealing** is to allow going down (of fitness) with a certain probability, depending on how bad is the down step.
Formalized version: Hill climbing with $p \neq 0$ of accepting a worsening solution $s'$ where $f(s) > f(s')$:
$$
 p = \exp\left( - \frac{f(s)-f(s')}{t} \right)
$$
Where $s$ and $s'$ are solutions and $t$ is a temperature.  
Core points of simulated annealing are:
- Estimate a probability to accept a worsening step
- Tweaking the balance between exploitation and exploration


### Tabu Search

The idea is having a list of **forbidden states**: the algorithm cannot visit again forbidden states. We need to validate a solution before exploring it, also it is useful to have a "tabu" list to track invalid candidates.
Tabu search operates in a **discrete space**.

### Iterated local search

A cleverer version of Hill Climber with Random Restart: the process restart form the point selected by `new_starting_position(global, last)`.

```python
def new_starting_position(global_,last):
	if global_ is None:
		return np.random.random((NUM_SETS,)) < .5
	else:
		return global_
```

We also need to decide when we need to restart (for example when there is no improvement in the last $n$ steps).
