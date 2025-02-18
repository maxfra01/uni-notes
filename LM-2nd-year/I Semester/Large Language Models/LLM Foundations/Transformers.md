# Architecture of Transformers

Let's begin considering a **simplified** **version** of the complete architecture:
1. Ignore all "residual connection"
2. Inputs/Outputs are simply tokens
3. Ignore add/norm terms
This is the resulting architecture after applying point 1-3: Feed forward networks are known, "Attention" block needs to be discussed.

![[Pasted image 20241002114645.png]]

In order to start generating tokens, we need to feed our encoder with our input sequence: it will produce the **code** (a state) composed by the same number of elements of the input sequence.
The code will be one of the inputs of the decoder, we also feed the decoder with a **BOS token** (this tells the decoder that we want to begin a sentence). Decoder will **output a first token** (tok1).
To generate the next token we feed the decoder with BOS and tok1: it will output tok2.
This process stops when the decoder produces a **EOS token**.
Note that this architecture is **stateless**: if we need to generate a $i$-th token we need to feed the decoder every time with $t_{i-1}, t{i-2}, \dots, \text{BOS}$

# Tokenization

Transformers need to process natural language, so we need a way to **split sentences into pieces**: this process is called **tokenization**.
Different strategies: split sentences in chars (see "case correction problem"), split sentences in words (see word2vec) or split sentences in **sub-words** (see fastText)

| Approach        | Pros                                                                                                                   | Cons                                                                                                                                                                                                                                           |
| --------------- | ---------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Character-level | 1. No out-of-vocabulary issues<br>2. Robust to misspelling and variations                                              | 1. Longer sequences<br>2. Slower and complex trai<br>3. Semantic information is harder to capture<br>4. inefficient for common words                                                                                                           |
| Word-level      | 1. Capture semantic meaning<br>2. Shorter sequence compared to character-level<br>3. Easier to understand              | 1. Out-of-vocabulary (OOV) issues for new or rare words<br>2. Does not leverage some information shared among words (e.g. prefixes/suffixes)<br>3. Larger vocabulary means memory inefficiency<br>4. Vectors for rare words are trained "less" |
| Sub-word level  | 1. Balance between approaches<br>2. Handle OOV<br>3. Compact vocabulary<br>4. Efficient for both rare and common words | 1. Common words are represented by fewer subwords w.r.t. rarer words<br>2. Requires defining a subword definition policy<br>3. This policy might introduce computational overhead                                                              |
In practice, the **sub-word approach is the most common**: the most used technique to split words is **Byte-Pair Encoding** (BPE).

### BPE (Byte-Pair Encoding)

The idea is to use a **data-driven approach** to choose the best tokens: in practice we don't define tokens, but we use a **corpus** (train data) to create them.
Common words should be associated to a unique token.

The algorithm is the following:
1. At the beginning, the corpus is initially encoded with 1 character (1 byte)
2. We count the frequency of each **pair** of subsequent tokens in the corpus
3. The most frequent pair of tokens are combined into a new token
4. We replace all pairs with the new "combined" token
5. Repeat point 2-4 until a certain number of token is reached

With this technique the number of tokens used to represent the corpus decreases as we
increase the number of tokens used:

![[Pasted image 20241002123556.png]]

Typically, **10’s to 100’s of thousands of tokens** are used: representing a text with fewer tokens is desirable but tokenizers should avoid being overly for the adopted corpus.
In same case we define **special tokens**: we already seen BOS and EOS, but we can also define **SEP** (for separator) and **CLS** (for classifier).

### Positional encoding

So far, we have seen that the learning process maps each token to a vector, but this is not ideal: **positions are important** in sentences.
We will add a **positional encoding** to each token vector.

![[Pasted image 20241007084510.png]]

In this way, each vector is altered according to the position in the original sequence.
It's not possible to avoid the positional encoding, because the "attention" mechanism does not understand sequences.
How to choose a positional encoding?

In the **AIAYN** paper, positional encoding vectors are defined in the following way:
$$
PE_{pos,2i} = \sin\left( \frac{pos}{10000^{2i/d}}_{\text{model}} \right)
\quad \quad PE_{pos,2i+1} = \cos \left( \frac{pos}{10000^{2i/d}}_{\text{model}} \right)
$$
![[Pasted image 20241007085444.png]]

Let's consider the second word and the first element of PE: $i = 0$, $\text{pos} = 1$, $PE = 0.8451$
PE vectors generated in this way are unique for each possible position. At least for the first 60000 positions

If position 1 and position 2 are close, then we want PE vectors to be close: **trigonometric functions** ensure this behavior.
We can plot cosine similarity and we get the following result: along the diagonal we can see that each vector is very similar to its neighbors.

Some models do not use sinusoidal PE: instead they learn positional embeddings during the training phase (along with the weights). (GPT-2  for example)

![[Pasted image 20241007090928.png]]

In the GPT-2 cosine similarity we can notice that, for vector 200, there is a weird behavior of the similarity.

# Attention mechanism

In Transformers, **attention** mechanism are widely used.
In short, it takes **independent** input vectors, mixes them up and returns a **contextualized** version of those inputs.

![[Pasted image 20241007092337.png]]

Each output is computed by a weighted sum of each input token. We are defining "how much" of each input token is useful. Considering the $i$-th output we can define it as:
$$
\text{out}_{i} = \sum_{j} \text{attn\_weight}(i,j)f(in_{j})
$$
The attention mechanism need to be able to figure out the attention weights on its own.
In other words, we can recast the concept of attention as a "weighted dictionary lookup".
The attention layer has three different inputs:
- **Queries**: the values to be looked up
- **Keys**: the values to be matched against the queries
- **Values**: The values associated to each key
To compute similarity between queries and keys, we can use **dot products** (it's unbounded, so we add a softmax).

![[Pasted image 20241007093127.png]]

- $Q$ contains $N$ queries, each being a $d_{k}$ dimensional vector
- $K$ contains $M$ entries, each being a $d_{k}$ dimensional vector
- $V$ contains $M$ entries (one value for each key), each being a $d_{v}$ dimensional values

In the original transformers, the final attention is defined as:
$$
\text{Attention}(Q,K,V) = \text{softmax}\left( \frac{QK^T}{\sqrt{ d_{k} }} \right)V
$$
- $QK^T$ computes all dot products
- $\sqrt{ d_{k} }$ it's only a scaling factor to prevent the growth of the variance of $QK^T$
- Softmax scales the result in the $[0,1]$ range
- Multiplying by $V$ creates a weighted sum of entries in $V$

So far we have introduced $Q,V,K$, but where do they come from?
They come from the input sequence: the attention inputs are transformed with a linear layer (no activation function) to produces the values.
$W^Q,W^K, W^V$

![[Pasted image 20241007095641.png]]

### Types of attentions

In transformer's architecture we use attention in three situation:
- Yellow: Encoder self-attention
- Blue: Decoder masked self-attention
- Magenta: Encoder-decoder cross-attention

![[Pasted image 20241007102456.png]]

| Type                            | Description                                                                                                                                                                                                                                              |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Encoder self-attention          | Classic attention mechanism, each token sees all other tokens in the input sequence, the same input sequence generates query keys and values (hence the name, **self**)                                                                                  |
| Decoder (masked) self-attention | One the attention block used by the decoder, works on the output tokens, it introduces the property of causality (each token can only see the past). To do so, we mask all invalid attention weights:<br>![[Pasted image 20241007103149.png]]            |
| Cross-attention                 | This attention is used by the decoder to receive information from the encoder. Keys and Values come from encoder's output, queries come from decoder's sequence. Each decoder token can see (attend) all encoder tokens, so there is no need for masking |

### Advantages of transformers

1. **Parallelization**: transformers can process all token simultaneously
2. **Long range relationships**: With the attention mechanism we have a better understanding of the entire sequence of tokens
3. **Better memory**: Since there is no sequential input, "forgetting" issues do not occur

###  T5 (Text to text transfer transformer)

This model is a vanilla encoder-decoder transformer. It is basically a simple framework that is able to perform multiple task. In order to perform a specific task, we need to insert a **prefix** that select the task itself.

![[Pasted image 20241009114547.png]]

The task instruction must be precise, otherwise the model provides garbage.

### Bert

This model is an example of **encoder-only** transformer. BERT (Bidirectional Encoder Representations from Transformers) it's pre-trained on two **self-supervised tasks**: "masked LM" and "Next Sentence Prediction".
The main feature of Bert is the **bidirectional attention mechanism**: all tokens can attend each other tokens, but this requires being careful with the task definition.

![[Pasted image 20241009115832.png]]

Bert supports pairs of input sequence (A,B), separated by the **SEP** token. Input sequence begins with **CLS** token. **Segment embeddings** are associated to each token to enrich their value.

1. In Masked LM, some tokens are hidden, and the model should predict the correct word.![[Pasted image 20241009120823.png]]

2. In **Next sentence prediction** we have two sentence as input. The model predicts if sentence B is the correct sentence after sentence A. In this case the CLS token is used at the beginning of the input. It will contain the result of the prediction.


### GPT (Generative pre-trained transformers)

GPT is an example of **decoder only** architecture: in this type of transformer, the input is a sequence, and the model tries to extend it. The rest of the output is generated in an autoregressive manner.

In GPT families, the model is **fine-tuned** on a specific supervised task (next word prediction): to generate next token, we use **greedy sampling**, so at every step we choose the token with the highest probability.

# Sampling approaches

The goal of a transformer is **next token prediction**: if the output is a probability distribution across all known tokens, then we need some **policy** to choose the actual predicted token.

### Greedy

At each step we select the token with the **highest** probability. That is a **deterministic** approach and could lead to repetitive or predictable text.

![[Pasted image 20241104152935.png]]

### Beam search

Start with the top k sequences (**beam width**), we expand each sequence by one token
at a time.We keep only the **k most probable** sequences after each step
- Repeat until stopping criterion
- Explores some options before “committing” to a sequence
Still a **deterministic approach**

![[Pasted image 20241104153218.png]]

### Random sampling

At each step we choose a random token, could lead to non sense sentences.

### Top K

We sample from the **top-k most probable words**. This avoids having low-probability
words showing up from time to time.

![[Pasted image 20241104153443.png]]

### Top P

Similar to top-k, we sample from a subset of tokens: the subset is defined to cover a
fraction p of the probability mass.
This provides an adaptive pool of candidate tokens:
- For high entropy distributions, there are more tokens to choose from
- For low entropy distributions, there are fewer tokens to choose from

![[Pasted image 20241104153608.png]]

### Temperature sampling

Like random sampling, but we use a version of softmax with a temperature $T$
Currently, we always assumed $T=1$.
- High temperature ($T>1$) flattens the probability distribution (**high** **entropy**)
- Low temperature (𝑇 < 1)  sharpens the probability distribution (**low** **entropy**)
General formula:
$$
y_{i} = \frac{\exp\left( \frac{z_{i}}{T} \right)}{\sum_{j}\exp\left( \frac{z_{j}}{T} \right)}
$$
Note that with $T \to 0$ then we are dealing with deterministic greedy sampling.

![[Pasted image 20241104153931.png]]
