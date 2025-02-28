# Introduction

**Prompts** are sequences of instructions and context that are provided **as input to a Language Model,** to achieve a desired task.
**Prompt Engineering** is the practice of developing and **optimizing prompts**, to efficiently use LMs for various application.

![[Pasted image 20241113124305.png]]

Main **principles** of Prompt engineering:
-  **Specificness**: the more the prompt is specific, the more focused output will be obtained
- **Step-wise approach**: break large tasks into small chunks
- **Iterate and improve**: re-work the inputs with iterative interaction with the LM

Let's consider the structure of the basic prompt:

![[Pasted image 20241113124516.png]]
### Techniques

**Priming**: The practice of providing some initial input to the model before generating a response. The initial input is intended to guide the model toward generating a more
coherent response.

**Tabular format prompting**:  Use tabular format for clear organization and presentation of data. It makes it easier for the user to analyze and comprehend the output.

**Fill-in-the-blank prompt**:  Allows the user to focus on a specific aspect of a sentence, text or idea.

```ad-example
title: Fill in blank prompting
Create a story about a detective in [choose a setting: e.g., a futuristic city, a
small coastal town, a bustling international airport]. The detective, [choose
a name for the detective], is known for [describe a characteristic or skill:
e.g., keen observation skills, a knack for technology]. The story begins with
[set the scene: e.g., an unusual disappearance, a cryptic message left
behind]. As [detective's name] delves into the case, they use [mention a
tool or method: e.g., advanced gadgets, old-fashioned deduction] to
uncover clues. Through [describe a key location or detail in the
investigation], [detective's name] discovers [reveal a twist or critical piece
of information]. However, they face [introduce a challenge or obstacle: e.g.,
a powerful adversary, a race against time]. How does [detective's name]
navigate this situation? Tell the story of how they solve the mystery.

```

**Perspective prompting**: The practice to provide a more comprehensive view of the topic at hand. This helps in making more informed decisions and have a wider understanding of a complex issue.

![[Pasted image 20241118090002.png]]
**RCG (Role, Goal, Context)**: Divide the prompt into 5 different fields, specifying the role, the 
result, the goal, the context and constraints.

### Chain of thought

![[Pasted image 20241118093322.png]]
### RAG (Retrieval Augmented Generation)

![[Pasted image 20241118095140.png]]

