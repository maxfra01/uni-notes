
# Six students problem

Six students (3 from computer engineering, 3 from data science), they need to get to point A to point B, on a tandem (2 person at a time)
"CCCDDD" -> ""  
Constraint: No more C > D anytime or the evening is ruined.

First step is **modeling**: composed by abstraction, which is simplifying the problem by focusing on its key element , and **representation**, which consists in expressing the problem in a formal language. There is also **Simplification**, aka making assumptions or approximation
Some terminology:
- **Problem** space (aka real word)
- **State** space (aka result of modelling phase)
- **Solution** space (aka viable states from state space)

In our problem the state space is the number of people at home (point A) and at the pizzeria (point B), but we also need to model the tandem. Remember that the state contains all needed information!
A correct state can be (friends at A, friends at B, position of tandem)
Once we model all correct states, we can recast this problem as a shortest path problem between state (CCCDDD*, .) -> (. , CCCDDD*)

Modeling is an essential phase, where we identify **key items** and ignore irrelevant thing and **formalize** the problem. We are allowed to make valid assumptions to make the problem tractable.
For the solving phase we can incrementally build a solution (non necessarily from beginning), or improve an existing solution.

# Knapsacks

Many variants of the classical problem:
- Multidimensional knapsack problem: add new constraint (for example volume of objects)
- Multiobjective knapsack problem: objects are evaluate with multiple criteria (money, sentimental)

Solving this problem with:
- Random mutation hill climber: with a simple tweak (either we take or don't take an object) it leads to a **steady state**.
- RMHC with a more powerful tweak: there is a small probability of making multiple small changes in the solution

