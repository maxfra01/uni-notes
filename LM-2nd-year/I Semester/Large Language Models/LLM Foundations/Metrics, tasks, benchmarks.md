# Metrics

### Perplexity

Perplexity is a metric that quantifies **how uncertain the model is in predicting the (correct) next word**.
- High perplexity: the model is uncertain about the “correct” next word
- Low perplexity: the model is certain about the “correct” next word
For example if we are predicting the next word and the probability of a word is $p(\text{cat})=0.25$ it's as if the model is confused between 4 words.

The perplexity over a sentence $w_{1},w_{2}\dots w_{N}$ tokens is computed as follows:
$$
\text{PPL} = \exp\left( -\frac{1}{N} \sum_{i}^N \log(p(w_{i}|w_{< i})) \right)
$$
If the model is **always confident**, then we have $\text{PPL}=1$.

![[Pasted image 20241016123517.png]]

### BLEU

BLEU (Bilingual Evaluation Understudy) is a metric used to evaluate a generated sequence, when a reference one is available.
If a model generates a very short sequence (w.r.t. the reference), it is easier to obtain a larger precision!
$$
\text{BLUE}_{n} = \text{BP} \cdot \left( \frac{1}{n} \sum_{i} \log(\text{precision}_{i}) \right)
$$
Where $\text{BP}$ is called **Brevity penalty**:

![[Pasted image 20241021103528.png]]

This type of metric is useful only if **we need an exact match**.
However:
- BLEU does not care about the **semantic** of the results
- No consideration about **fluency or meaning**
- **Words order** ignored

### Other metrics

- **ROUGE** (Recall-Oriented Understudy for Gisting Evaluation): more focus on recall, rouge can choose between a family of function
- **METEOR** (Metric for Evaluation of Translation with Explicit Ordering): addresses some problems of BLEU, includes both precision and recall

### BERT Score

This technique starts tokenizing a **generated sentence G** and a **reference sentence R**. After that we compare the tokens from G and R (for example with cosine similarity).
$$
\text{precision} = \frac{1}{|G|} \sum_{i} \max_{j} \text{sim}(G_{i}, R_{i})
$$
$$
\text{recall} = \frac{1}{|R|} \sum_{j} \max_{i} \text{cossim} (G_{i}, R_{i})
$$
![[Pasted image 20241021104534.png]]


- BERT score "solves" the semantic similarity problem
- Still, doesn't consider order explicitly
- BERT score relies on an external model


# Tasks

### Text completion

 The model is given an incomplete sentence or passage and must figure out what a plausible continuation is.
- The model either generates the correct continuation of the text,
- Or, it chooses the best option among a pool of options

A useful database for this task is **LAMBADA**: this is a collection of narrative passages, it's designed so that humans can guess the word if they read the entire passage and not just the last sentence.
Other datasets are made by short stories and different possible endings.

### Q&A

We provide a model a question and we expect an answer. Two possibilities:
- Open-books: allowing the model to "search" the answer in a set of texts
- Closed-books: measures what the model already knows
We can also identify different types of answer: extractive, abstractive, multiple choice.

Useful datasets are SQuAD, TriviaQA.

### Translation, summarization

-  Generate the translation of a sentence, and evaluate the quality of the result
- Generate a summarized version of an original text (e.g., news article)

### Natural Language Interface (NLI)

Given a premise, NLI is the task to **determine if an hypothesis** is True, False or Undetermined (Entailment, Contradiction, Neutral).
Useful datasets are **Stanford NLI** and Multi-genre NLI.

### Other tasks

| Task                      | Description                                                                                                                                                                                 |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Grammatical acceptability | Can models detect whether sentences grammatically correct or not? Dataset CoLA                                                                                                              |
| Text classification       | Classifying sentences into categories based on the contents                                                                                                                                 |
| Mathematical reasoning    | Datasets MATH and GSM8k                                                                                                                                                                     |
| Commonsense reasoning     | Commonsense reasoning is the ability to make inferences based on everyday knowledge of the world: cause-effect, social norms                                                                |
| Ambiguity resolution      | Given a sentence with an ambiguous pronoun, can the model<br>understand the entity, based on context?<br>- The trophy didn’t fit in the suitcase because it was too big.<br>- What is “it”? |
| Causal reasoning          | Determine cause-effect relationships: datasets COPA, ATOMIC                                                                                                                                 |
| Synthetic tasks           | We can generate specific tasks synthetically: the task may be easy to generate, but non-trivial for the models to solve                                                                     |

# Benchmarks

Purpose of benchmarks is to provide an **exhaustive evaluation of LLMs**.

| Benchmarks | Description                                                                                                             |
| ---------- | ----------------------------------------------------------------------------------------------------------------------- |
| GLUE       | "General Language Understanding<br>Evaluation" provides a single number score combining performances from various tasks |
| SuperGLUE  | Same as before, but introduced more difficult tasks                                                                     |
| MMLU       | "Massive Multitask Language Understanding" focused on question answering                                                |

Since datasets and benchmarks are public data, it may happen that they end up in the training corpus of LLMs (Intentionally, or not…).  The continuous improvements across tasks may be somewhat related to this kind of **contamination**.
For closed source models, this cannot be proven.
Contamination can occur at various levels.