# GPT family

### GPT-1

In 2018, OpenAI releases GPT-1 model. It is a decoder-only model, pre-trained on the text generation task: training was performed with unsupervised approach (on 5 GB of text); fine-tuning process was supervised.

![[Pasted image 20241014104324.png]]

GPT-1 was fine-tuned for: **natural language inference**, **question answering**, **semantic similarity**, and **classification**.
- Architecture has 12 layers decoders-only, 12 heads each.
- Learned positional embeddings
- Context size of 512 tokens
- **117 million** parameters 

### GPT-2

The next year, in 2019, OpenAI releases GPT-2 model. It is basically the first one but trained with more data. They didn't use fine-tuning, only unsupervised training.
Even without tuning, GPT-2 was able to solve tasks it was not explicitly trained on!
OpenAI notices that increasing the model capacity improves performance in a log-linear fashion across tasks.

Concerns start to emerge:
"Due to concerns about large language models being used to generate deceptive, biased, or abusive language at scale, we are only releasing a much smaller version of GPT-2 along with sampling code”.

The text used for training is 40 GB of text. Fine-tuning was removed. Context size was doubled (1024 token).
The total number of parameters is **1.5 Billion**.

### GPT-3

In January 2020 OpenAI releases a new paper that tries to give explanation on growth trends in language models:

![[Pasted image 20241014105553.png]]

Performance depends very weakly on other architectural hyper-parameters such as depth vs. width (number of layers vs embedding size).
Empirically they found the following result:
$$
N \propto C^{0.73} \, \, \, B \propto C^{0.24} \, \, \, S \propto C^{0.03}
$$
Where $N$ is the model size, $B$ Batch size, $S$ number of steps, $C$ computational power
As the computational budget C increases, it should be spent primarily on larger models, without dramatic increases in training time or dataset size.

In June 2020, OpenAI releases GPT-3, with **175 Billion** parameters.
Datasets used for training: **CommonCrawl** (low quality data), WebText2, Wikipedia ecc... 

Similar to GPT-2, there is no fine-tuning. There is **in-context learning**, which is basically provide the task in natural language:
- Zero-shot: no information is provided to the model
- One-shot: one input/output example is provided in the prompt
- Few-shot: more than one example are provided

![[Pasted image 20241014111625.png]]

This plot shows that few-shot model is comparable to **fine-tuned SOTA**: this result is remarkable because tells that scaling the model allows not fine-tuning on task-specific
datasets and still get competitive results!

# Bigger model

Following this trend, many companies develop models with 100+ Billion parameters (GPT-3 was the first).

- **Jurassic-1**, 178 billions parameters
- **Gopher**, 280 Billion parameters
- **Megatron-Turing NLG**, 530 Billion parameters

DeepMind releases a paper with the following claims:
- “current large language models are significantly under-trained, a consequence of the recent focus on scaling language models whilst keeping data constant”
- “for every doubling of model size the number of training tokens should also be doubled”
- **Chinchilla**, a new correctly-sized model, outperforms larger ones!

![[Pasted image 20241014112813.png]]

From the plot above we notice the dashed line, i.e. the trend OpenAI tells to follow.
The other lines are different approaches in building models.

### Approach 1 (Fix model size)

Starting analyzing the relation between loss and FLOPs, we can retrieve a new relation between FLOPs used to train the model and the number of parameters.

![[Pasted image 20241016114808.png]]

### Approach 2 (Fix computing power)

Fixing the computational power, we can train different model with the same computing power and we retrieve the first of the three plot.
Again we can retrieve a new relation between FLOPs and parameters. Again it can be shown that Gopher only needed 63 B parameters

![[Pasted image 20241016115111.png]]

### Approach 3 

![[Pasted image 20241016115659.png]]

# Chinchilla

In the same paper about under-trained models, DeepMind presented **Chinchilla**, which is a new model with **70 B parameters**. It has the same FLOPs of Gopher but is four times smaller. On the other hand, **Chinchilla is trained on 1.4 T tokens**, which are four times the tokens of Gopher.

The interesting part is that **Chinchilla outperforms both Gopher and GPT-3** (in some tasks).
This broke the "bigger model" trend, and smaller LLMs return!
Since smaller models could perform better, then LLMs became more accessible!

![[Pasted image 20241016120614.png]]

Interesting Open-source models are:
- **Llama**, released by Meta
- **GPT-Neo/GPT-J**, by EleutherAI
- **Mistral**, by MistralAI
- **GLM**, by ZhipuAI, General language model
- **Falcon**, by Technology Innovation Institute, first open model based on **Mamba**, which is a new paradigm for transformers
