# Language Models introduction

### Definition of Language model

A **Language model** is a **probabilistic model** of a **natural language**: in other words, this model produces a probability for any sequence of words $w_{1},w_{2},w_{3}, \dots$
So, each possible sentence in the natural language is associated with a probability (even sentences with grammatical errors or incoherent).
We divide the **natural language space** into regions that can be more "plausible" of "non plausible". Of course all possible sentences probability sums to 1.

To be more precise, the formal definition is:
$$
P(w_{1},w_{2},\dots) = \Pi_{t=1}^T P(w_{t}|w_{t-1},w_{t-2},\dots, w_{2},w_{1})
$$
We are considering the probability of a word $w_{t}$ following a bunch of other words $w_{1},w_{2},\dots,w_{t-1}$

### N-grams models

The **n-grams models** make a simplifying assumption (**Markov assumption**) that states:
$$
P(w_{t}|w_{t-1},w_{t-2},\dots) \approx P(w_{t}|w_{t-1},\dots, w_{t-n+1})
$$
In those models the next word $w_{t}$ depends only on the $n-1$ previous words: the small window of $n-1$ previous words is called **context**.
Given some training text, we can count the n-grams occurring in the sentences: using the frequencies we can estimate probabilities.
Once we have the probabilities we have a language model!
The model can be used to **estimate or generate** new sentences: starting from a given word we can use our probabilities to generate a new sentence. This approach is called **autoregressive generation**.

If $n=2$ we can make a simple example: let's consider the following sentences:
- "The cat chased the mouse happily"
- "The mouse ate the cheese"
Using the strategy described above we get the following 2-gram:

![[Pasted image 20240923153344.png]]

Let's see an example where we estimate the probability of a new sentence.
We want to compute the probability of "the, cat, chased, the, cheese": Markov assumptions tells us the this can be expressed as: (we assume the probability of the first word is 1)
$$
P(\text{the, cat, chased, the, cheese}) \approx P(\text{the}) \cdot P(cat |the) \cdot P(chased|cat) \cdot P(the | chased) \cdot P(cheese|cat)
$$

If we want instead to generate a new sentence, we can choose a starting word. Then we simply consider the next word that has a higher probability (in case of tie, we can randomly choose) given the context.

Obviously those models have limitations:
- **Data sparsity**: As $n$ increases, the number of possibile n-grams grows exponentially. N-grams works well only for small context size.
- **Context limitation**:  the model chooses the next word using a small **local** context, we will see that longer sentences require a bigger context
- **Lack of semantic**: same words in different contexts are treated in the same way

### History of language model

LM's ideas is not recent: in the 90's researchers used the technologies they had to build models, but they performed poorly.

We can treat the generation of the next word as a classification function: many models can be employed to satisfy this task. In the 2000s **neural network** shown to be quite good performing this task.

In 2019 results were much butter, for example GPT-2 used a **decoder-only transformer** architecture, a "big" model pre-trained on lots of data.

Recent trends in language models has been to:
- Fix the architecture
- Increase the size of models, feeding them more data, using more context, but this approach cannot scale unlimited times


