# Word embeddings

**Word embedding** is a technique that map each word into a **dense vector representation**.
Each word is mapped to a vector of real numbers (high dimensionality), that allows to capture **semantic meanings** and **relationship** between words.
For example, words with similar meanings have similar representations, while words with similar connections (relationship) are linked via similar transformations.

![[Pasted image 20240930084905.png]]

### One hot encoding

Before word embedding, people used **one-hot encoding**: let's assume we have a vocabulary with $W$ words. For each word we create a $W$-dimensional sparse vector, then for the $i^{th}$ word, all dimensions are set to zero, except for the $i^{th}$, which is 1.
- Vectors are **sparse**: this leads to scalability issues (curse of dimensionality) and also space is not used efficiently
- Vectors are **orthogonal**: there is no preservation of semantic similarity, all words have the same distance.

### Distributed  representations

We've seen that one-hot encoding is a type of local representation (unique, isolated identifier). By contrast, a **distributed representation** aim to distribute the information across several dimensions. We **let models** learn these representations:
Consider the following task:

![[Pasted image 20240930085832.png]]

The model needs to estimate the following probability:
$$
P(x_{i} = w) = P(x_{i}=w| x_{i-1}, x_{i-2}, x_{i+1}, x_{i+2})
$$
The main difference between this task and classic [[Language Models]] is that here we are considering both following words and the previous ones.
In order to solve this problem we need to:
1. Assign each word to a vector (at first, vectors will be random)
2. Aggregate all vectors into a single one, called $h$ **context vector** (to compute $h$ we just sum vectors)
3. Assign each candidate output word to a vector (also random at first)
4. Compute all possible distances between the context and the candidates (via dot products)
5. Find the best-suited word for the context
6. Is it the correct word?
7. Adjust the model via gradient descent

We can represent all possible words with a matrix $W_{in}$ (size: $d \times V$) where $d$ is the embeddings dimensionality and $V$ is the total number of words.
Output words can be also stacked in a $d \times V$ matrix: $W_{out}$
The **entire context** (which words are valid) can be represented as a "bitmap" of words $e$ (size: $V \times 1$)
The sum of words inside the context can be computed as $$h=W_{in} e^T$$ (size: $d \times 1$), here $e$ acts as a selector of the correct columns (words) of $W_{in}$
We can compute the vector of similarities $\tilde{p}$ as:
$$
\tilde{p} = h^T W_{out}
$$
$\tilde{p}$ describe the similarity between each output word and the context, we can normalize the similarities between 0 and 1 with a softmax function, then we can use cross-entropy to update the weights of the model.


