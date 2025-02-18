# Searching for a policy

We start with a difference between **Learning and Planning**: in a learning process, environment is unknown, agents interact with the environment, agent improves its policy (kind of trial and error). In a planning phase a model of the environment is known.

A **policy** is something that determines the action the agent should take in each state.
A **deterministic policy**: maps each state to a specific action with a probability: $$
\pi(s)=a
$$A **Stochastic policy**: maps each state to a probability distribution over actions:
$$
\pi(a|s)=P[A_{t}=a| S_{t}=s]
$$
When working with "best policy" we need to focus on our objectives: most common case is **handling worst-case scenario**, i.e. minimizing the potential losses or worst-case outcomes (example, Natural Disaster).
A different objective is the **maximization of returns**: portfolio management for example could be an application. It's also what we do when doing reinforced learning.
A trade-off between this approaches is **robust optimization**, suitable for a range of scenarios.

# Reinforcement learning

A RL model is composed of two main entities: **agent and environment**. An agent perform an **action** that changes the environment. then it provides a **reward** and a change of **state**.
We are working in a **discrete scenario**.

![[Pasted image 20241125124025.png]]
In this context, the problem characteristics are:
- **Sequential**: solution is a sequence of steps
- **Discrete**: the alternative actions can be enumerated

Another distinction is based on the length of the simulation:
- **Episodic task**: we have the certainty that at some point he simulation will end (e.g. playing tic tac toe)
- **Continuing task**: We have no guarantee that the simulation will end

The **goal** is to **maximize the expected cumulative reward**. The agent is rewarded for its actions: the reward could be sparse, uncertain or delayed.
More formally, at each step:
1. The agent receives a representation of the state $s$
2. The agent decides an action $a$
3. The agent receives a reward based on a **reward function** $f_{r}$

The **State is a function of the history**:
$$
f(H_{t})=S_{t+1}
$$
The environment state $S_{T}^E$ is the true state, but it might be not fully observable or contain irrelevant information.

A **State is Markov** if:
$$
P[S_{t+1}|S_{t}]=P[S_{t+1}|S_{0},S_{1},S_{2}\dots S_{t}]
$$
i.e. the future is **independent from the past, given the present**.


Modern RL approaches are **Model based** or **model free**. In the Model-based the **dynamic of the system is known**, so the model allows to predict what the environment will do next. 
### Markov Processes

A Markov process is a sequence of random states with the Markov property. Is a tuple $\langle S, P \rangle$ where S is a finite set of states and P is the probability of state transition.

![[Pasted image 20241201180331.png]]
An **Episode** is just a finite **sequence of actions**, for example C1 C2 C3 Pass Sleep

### Markov Reward Process

We can add the **reward** to Markov Reward process so we can describe it with $\langle S, P, R, \gamma\rangle$ where $R$ is the reward and $\gamma$ the **discount rate**.

![[Pasted image 20241201180743.png]]

The goal $G_{t}$ can be described as the total discounted reward form time-step $t$, so what I expect from $t$, then $t+1$ and so on:
$$
G_{t} = \sum_{k=1}^\infty \gamma^k R_{t+k+1}
$$
The expected return starting from state s is the **state-value function**, i.e. the value that I am supposed to get if I am in state $s$:
$$
v(s)=E[G_{t}|S_{t}=s]
$$
![[Pasted image 20241201181417.png]]

A **Bellman equation** describes the following scenario: the state-value function can be decomposed into two parts: an **immediate reward** $R_{t+1}$ and the **discounted value of successor state** $\gamma v(S_{t+1})$:

![[Pasted image 20241201181550.png]]
So in other words we can calculate the state-value function from the following formula (recursive nature of the expression):
$$
v(s) = R_{s} + \gamma \sum_{s' \in S} P_{ss'}v(s')
$$

In the example we do not calculate anything, we are just checking if the results are correct.
![[Pasted image 20241201181751.png]]
To solve the Bellman equation we could use matrix form but this is suitable only for "few" states, because the complexity for $n$ states is $O(n^3)$. There are several alternatives which are Dynamic Programming, Monte Carlo search...

### Markov Decision Process

Reinforcement learning utilizes the **MDP** framework to model the interaction between a learning agent and its environment. In this framework, the interaction is characterized by states, actions, and rewards.
MDP are **fully observable**.

A **Markov decision process** (MDP) is described with the following tuple.
$$
\langle S, A, P,R, \gamma \rangle
$$
- $S$ is a set of states, called **state space**
- $A$ is the space of the **possible actions** (actions space)
- $P_{a}(s,s')$ is the **state transition probability** from state $s$ to $s'$, if we choose action $a$
- $R$ is the **reward** function
- $\gamma$ is the discount rate

![[Pasted image 20241201182209.png]]

A **Policy** $\pi$ is a distribution over actions given states:
$$
\pi(a|s) = P[A_{t}=a | S_{t}=t]
$$
A policy **fully describes** the behavior of an agent. Policies are **time-independent** and could be deterministic or stochastic.

In the case of continuing tasks, at some point the importance of the reward starts getting lower and lower. We need, after some time steps, a way to discard those lower rewards and this is called the **discount rate**.
So the goal of a task become the **maximization of the expected discounted return of rewards**:
$$
G_{t} = \sum_{k=1}^\infty \gamma^k R_{t+k+1}
$$

We call **state-value function** $v_{\pi}(s)$ for a given policy the **average expected reward starting from state $s$ and following the policy until the end**. 

![[Pasted image 20241128134314.png]]

![[Pasted image 20241201182749.png]]

The **action-value function** is described by the following formula, where $q_{\pi}(s,a)$ is the quality value for policy $\pi$. In other word, if I am in state $s$ and I do action $a$, this is my expected reward.

![[Pasted image 20241201182838.png]]
In each state we can describe the bellman equation as the expected value of the actual reward plus a discounted value of successor state. Again same idea, but from a recursive view.

(again, we are not calculating, just checking the result)
![[Pasted image 20241201183124.png]]

The idea of **Optimal value function** is described as the expected return starting from $s$ following the **best available policy**:
$$
v_{*}(s) = \max_{\pi} v_{\pi}(s) = E[G_{t}|S_{t}=t]
$$
The optimal action-value function $q*(s,a)$ is the **maximum action-value function over all policies**:
$$
q_{*}(s,a) = \max_{\pi}q_{\pi}(s,a)
$$

![[Pasted image 20241201183454.png]]
![[Pasted image 20241201183641.png]]

For any Markov Decision Process there **exists an optimal policy $\pi_{*}$** that is better than or equal to all other policies. All "**optimal policies**" achieve the optimal value function and the optimal action-value function.

Bellman optimality equation is **non-linear**, there are no closed form solution, so we have many **iterative solutions**: Value Iteration, Policy Iteration, **Q-Learning**.

### Policy Improvement

Considering a deterministic policy $a$ we can improve it in a greedy way:
$$
\pi'(s) = \arg\max_{a} q_{\pi}(s,a)
$$
This also improves the value function. At some point, the improvement will stop, so we reached the bellman optimality.
The optimal policy $\pi$ can be found by iterative application of Bellman optimality backup.

### Synchronous DP

Given m actions and n states. we can calculate $v_{\pi}(s)$ with complexity $O(mn^2)$ per iteration, and calculate $q_{\pi}(s,a)$ with complexity $O(m^2n^2)$ per iteration.
Synchronous value iteration stores 2 copies of value function, while in-place value iteration only stores one.

# Monte Carlo RL (Model-free)

In this scenario we have no knowledge of MDP transitions or rewards, but the system is MDP. This is a frequent assumption.
If there is something Markov based we could try to **learn directly from episodes**: the idea is to take an action, estimate a metrics, update the state and get a reward.

First-visit MC policy: the first time $t$ that state $s$ is visited in an episode we increment counter: $n_{s} = n_{s}+1$ and $S_{S}=S_{S}+G_{t}$ where the value is estimated by mean return $\bar{S}_{S}=S_{S} / n_{s}$

Every-visit MC policy: Every time $t$ a state $s$ is visited we increment the counters, same as before, but we do it at every time-step.

Incremental MC update: at each state using an 'incremental mean style' formula:
$$
S_{S} = S_{S} + \frac{1}{n_{s}}(G_{t}-S_{S})
$$
# TD Learning (temporal difference)

Temporal-Difference Learning: in this scenario we have no knowledge of MDP transitions or rewards and the idea is to updates a guess towards a guess.
In the TD 0 we Update value $S_{s_{t}} = S_{s_{t}} + \alpha(R_{t+1}+\gamma S_{s_{t}+1} - S_{S_{t}})$ 
Classic example of this is: imagine driving a car, and you narrowly missed an accident. Because of the 'panic' moment we update some policy based on that experience.
TD target.

In general TD exploits Markov property, while Monte Carlo does not.

