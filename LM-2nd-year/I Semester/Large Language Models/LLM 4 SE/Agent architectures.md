# Agentic Behavior

In **traditional LLM application**, the model follow a predetermined **control flow**, i.e. a fixed sequence of steps before and after LLM calls. In contrast to this method, the concept of **agentic behavior** refers to empowering an LLM to **decide its own control flow** to solve complex tasks. In other words, the LLM becomes an agent that can make decisione step by step.

![[Pasted image 20241125085538.png]]

Some advantages of agentic behavior:
- **Enhanced problem solving**: agents can solve more complex tasks by deciding their own flow of actions
- **Dynamic control flow**: More efficient and flexible processes, since there is no limitation from a "human user".
- **Improved efficiency**: the agent can avoid unnecessary computation when we gathered sufficient data.

# LLM Chains

A **LLM Chain** is a series of prompts and operations that **guide an LLM through a sequnce of tasks**, enabling more complex AI interactions.
It's just a "recipe for AI", that provides a structured workflow.

### Sequential Chain

An example could be the summarization of a text, that we divide into 4 steps:

![[Pasted image 20241125090543.png]]

The example above is what is called **sequential chain**, where the output of one step is the input of the following one. It is effective when dealing with subchains designed for singular inputs and outputs. It ensures a smooth and continuous flow of information, with each step
seamlessly passing its output to the subsequent step.

### Tree Chain

Complex structures require that an output from a single step acts as the input for multiple modules: that is what is called **tree chain**.

![[Pasted image 20241125090805.png]]
### Router Chain

In this scenario we have multiple subchains, each of them specialized in some kind of input. The we have a router that decides which subchain to use.

The **Router chain** consists in three components:
1. Router chain itself: the module that decides which subchain to call
2. Destination chains: All the subchains that the router can route to.
3. Default chain: the subchain called when the router can't decide

![[Pasted image 20241125091247.png]]

# Agent Architectures

An **agentic AI system** is capable of setting its own goals, planning actions to achieve those goals, and making independent decisions based on its understanding of the environment and task at hand. Unlike reactive AI, which simply responds to inputs, agentic AI can take
initiative, adapt to changing circumstances, and even learn from its experiences.

![[Pasted image 20241125091754.png]]
- The **Agent** is the **central decision-making entity** that orchestrates the use of tools, memory, and planning. It **determines** how to tackle a given task by selecting the most **suitable tool or planning strategy** based on the problem's complexity.

- The **Tools** (on the left) are specialized extensions that expand the agent's capabilities. For example, if tasked with planning a project timeline, the agent might use the Calendar to allocate dates and the Calculator to optimize resource allocation

- The **Memory** (on top) is divided into short-term and long-term. The first one stores temporary data or intermediate result, while the second one is used to store persistent knowledge for future reference

- The **Planning module** (on the right) is used to devise strategies to solve problems. This involves several the use or combination of several different strategies.

| Mode                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                    |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Reflection            | **Reflection** is a meta-cognitive process that allows the agent to evaluate its past decisions and actions to identify areas for improvement. This capability ensures that the agent learns<br>from its successes and failures.                                                                                                                                                                                               |
| Self-critics          | **Self-critics** enables the agent to analyze its own performance critically and suggest refinements. It operates as an internal feedback mechanism, helping the agent improve its reasoning and outputs.                                                                                                                                                                                                                      |
| Chain of Thoughts     | **Chain of thoughts** involves sequential reasoning to tackle<br>complex, multi-step problems. The agent progresses logically,<br>step by step, ensuring clarity and coherence in problem-solving.<br>This approach minimizes errors and makes the problem-solving<br>process transparent, facilitating debugging and iterative<br>refinement.                                                                                 |
| Subgoal decomposition | **Subgoal decomposition** is the process of breaking down a<br>large, complex problem into smaller, more manageable tasks<br>or milestones. This strategy enables the agent to approach<br>challenges in an organized manner, ensuring steady<br>progress toward the overall objective. This modular<br>approach ensures focus, reduces overwhelm, and enables flexible<br>adjustments if issues arise in individual subgoals. |
### Memory Model

![[Pasted image 20241125102740.png]]

- Memory retrieval aims to enhance decision-making accuracy by extracting valuable information pertinent to the current situation from an agent’s memory. This information encompasses various elements such as environmental perception, records of historical interactions, experiential data, and external knowledge

- Memory Refection is the process through which agents engage in self-improvement based on the perceived information and learned experience from historical interactions stored in memory. This process emulates the human practice of summarizing, refining, and reflecting upon existing knowledge, with the objective of enhancing the agent’s adaptability to new environments and tasks.

- Memory storage. The storage is typically realized through the use of natural language formed text, although it also encompasses multi-modal information such as visual and audio data. The storage format is determined by the specific nature of the task and the attributes of the data modality. By tailoring the storage format to the modality and task requirements, agents can more effectively utilize stored information, thereby enhancing their performance in diverse and complex environments.

- Memory Modification. When considering the similarity between new information and existing memories, it is crucial to determine the appropriate method of incorporation: whether to add new information, merge it with existing data, or substitute erroneous existing information.

### Knowledge Utilization

Knowledge utilization. Knowledge utilization focuses on integrating external knowledge (excluding memory information) into LLM based planning.
By leveraging up-to-date textual, visual, and audio data, LLMs enhance their ability to perform complex tasks accurately and contextually. Techniques such as retrieval-augmented generation and real-time web scraping allow these models to combine internal
capabilities with external information, thereby improving planning and decision-making processes.

![[Pasted image 20241125104235.png]]
