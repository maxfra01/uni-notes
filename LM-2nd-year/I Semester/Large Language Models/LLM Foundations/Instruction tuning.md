# Instruction tuning

So far, we have considered "next token" predictors, however complex LLMs like Chat-GPT need to both **follow instructions** and **provide helpful answers**. Our objectives can be summarized as **HHH** (Helpful, Honest, Harmless).
We also need to achieve **generalization**.

### T0 model

Models like T5 provides a general architecture that includes the task as a part of prompt, in this way the model leans to condition the answer based on the context (task-dependent). However T5 model has some limitations: it does **not generalized to new tasks** and it expects the prompt in a **specific format**.

T0 is a "T5-inspired" model, pre-trained on **masked LM task** and **fine-tuned** on a mixture of multitask Q&A pairs. We expect the model to be able to handle differently-worded prompt and to able to generalize to other tasks.
Dataset used for trained were split in two groups: fine-tuning and evaluation. All datasets for a task are either used for training, or for testing.

### Templates for prompting

It is generally difficult to obtain large datasets in the form of Question-Answer pairs, so to overcome the problem, the authors of T0 used **templates**
- **Each task was phrased as a question**
-  Multiple rephrasings for the same task
- Sometimes,inverting the task

### Results

As expected, **T0 shows remarkable improvement** in performance on **new tasks**, in **zero-shot** (no example provided), outperforming T5 and GPT-3.
This trend suggests that the model is getting overall better capabilities in new tasks by seeing more types of questions.

Another model presented by Google in 2022 is **FLAN** (Finetuned language net): once again instruction tuning substantially improves zero-shot performance on unseen tasks

![[Pasted image 20241023120059.png]]

# Model alignment

Making a model larger may improve its performance in next token prediction capabilities and zero-shot task generalization, as discussed.
However, models can still be **untruthful, toxic, not helpful**; in other words, models are **not aligned** with human preferences.

In _Learning to summarize with human feedback_ paper, authors highlights that LLMs are limited by **poor metrics** (e.g. ROUGE) not capturing information about the quality of outputs, and by **poor objectives** that don't specify how bad an error is. Also, during training, models aren't able to distinguish between high and low quality data.

The goal of the work is to improve alignment of LLM’s outputs with what humans actually think is useful. The proposed approach consists of three steps:
1. Collect **human feedback**
2. Train **reward** model
3. Fine-tune model to **learn "human"** feedback

### Collecting human feedback

We can test the **summarization task**:
- Initial summaries are generated according to some model
- A human annotator is presented with **pairs of summaries** for the same input
- The human chooses the **preferred one**.

![[Pasted image 20241023121142.png]]

```ad-note
title: Reinforced learning
In Reinforced Learning, a "**policy**" is a probability distribution across all possible actions, conditioned by the current state.For LMs, this corresponds to the output of a model, which is conditioned by the context. The policy, thus, is the probability distibution across all possible outputs.

```

We can see humans as **evaluation functions**:
- If we assume that the human produces a verdict we can model it with the following function: $$
f(p,s_{1},s_{2}): C \times C \times C \to \{0,1\}
$$
- Or, we can explicitly assert that humans produce a “reward” for each summary via $r(x)$, and produce a verdict 0/1 based on the largest reward: $$
f(p,s_{1},s_{2}) = 1(r(p,s_{1})> r(p,s_{2}))
$$
However humans have limitations too:
- **Cost & scalability**: collecting human feedback at the scale required for fine-tuning a model is impractical and expensive
- **Inconsistency**: different humans can be inconsistent. The same human may also be inconsistent across evaluations
- **Simplicity of feedback**: specific annotations may consider only one or few aspects of interest. Producing a more sophisticated score that objectively accounts for various aspects may be difficult for humans to do.

To overcome those limitations we could train a reward model, instead of using humans.

### Train a reward model

The reward model **predicts a scalar value**, proportional to the quality of the result.
If we are dealing with 2 summaries $s_{k},s_{j}$ of a post $p$ we can produce the scores:
$$
r_{j} = r_{\theta}(p, s_{j}) \, \, \, \, \, \, r_{k}=r_{\theta}(p, s_{k})
$$
We compute the loss of the reward model to **maximize the gap in rewards**: for instance, $r_{j}- r_{k}$ should be large if $s_{j}$ is the better result.

![[Pasted image 20241023122013.png]]

The major concern is the **quality of results**, i.e. can the reward model makes the same predictions as a human? Results show that larger models achieve better performance, getting close to a single human performance, but not as good as **ensemble of humans** approach.

### Fine-tune model on feedback

Starting from the original model (called "old"), we make a **copy** that we're going to optimize.
We now fine-tune the copied model that produces the copy policy.
Then, using the Reward model $r_{\theta}$ we can provide the feedback to the copied model.
We can frame the reward for the model as:
$$
R(x,y) = r_{\theta}(x,y) - \beta \log\left[ \frac{\pi_{new}(y|x)}{\pi_{old}(y|x)} \right]
$$
This terms, referred to as a **KL divergence**  quantifies how distant two distributions are.
Intuitively, if $\pi_{new}$ produces a sentence considered very unlikely by $\pi_{old}$ , the ratio will
be > 1, thus introducing a penalty.
In other words, it acts as a **regularizer** that **prevents the model from producing outputs that are too different from the original model**. The $\beta$ parameter controls the strength of the regularization.

What is a KL divergence?
It is tempting to maximize the reward $r(x)$, regardless of how distant from the original model we go. However $r(x)$ is a **proxy for human preference**, not the actual one.
Maximizing $r(x)$ ends up producing a very different from the original one.

![[Pasted image 20241023123444.png]]

### Results of alignment

Annotators are asked to choose the preferred version between the human-written and the model-generated summary.
Results show a **clear human preference** for the HF model w.r.t. all others, even better than the human-generated!

![[Pasted image 20241023123602.png]]

### InstructGPT

InstructGPT is an extended model created to address various tasks: it uses instruction tuning and human-written answers (not with templating!)
- It is more **truthful**, slightly **less toxic** than GPT-3
-  It's aligned to annotators it never learned from
- It generalizes to new tasks not in the fine-tuning datasets



![[Pasted image 20241023124401.png]]

### New trend

InstructGPT shows that pre-training alone is not enough for user-aligned behavior, and fine-tuning using high-quality, instruction-following data became essential.

1. **Pretrain a model on large quantities of (dirty) data**, scraped from various, potentially unreliable, sources. This stage focuses on learning general language patterns and knowledge
2. Collect **smaller, higher-quality datasets**: gather instruction-based datasets where human feedback refines the model’s responses.
3. Use RLHF to align models to user preferences

Note: The original Chat-GPT is based on InstructGPT (w/GPT-3 175B)


# Fine-tuning

### Fine-tuning vs Feature-based transfer

Fine-tuning is a simple and intuitive **approach to improve a model's performance on a new task**. In fine-tuning we **continue the training process** on new data: in this way all original weights can change. The process is powerful because it allows to change the behavior of our model, improving performances; however it could be resource intensive for larger model.

Another approach is called **feature-based transfer**: the idea is to use the original model as a feature extractor and we update only a part of the model.
To do so, we **freeze the backbone model** (so no gradients updates for this part), the we attach a **trainable head** which its weight can be updated and used for the new task.
It is less resource intensive, and works well only when the new task is slightly different from the original.

Fine-tuning and feature-based transfer introduce opposite beneﬁts/problems. As models grow larger and tasks diversify, we typically struggle to use one or the other approach. Other families of approaches emerged to overcome the limitations of the above mentioned techniques.

### Parameter Efficient Fine-tuning (PEFT)

PEFT is a **family of techniques** that tries to reduce computational cost required for the fine-tuning process, by reducing the number of parameters to be updated.
Among the others, there are:

### BitFit

**Bias-terms Fine-tuning** (BitFit) is a very simple sparse ﬁne-tuning technique, where only the bias terms of the model (or a subset) are modiﬁed. Usually bias-terms are a small fraction of model's total weights.
Studies shown that tuning only those biases is sufficient to get performance comparable to fine-tuning.

### Adapter layers

The idea is to add a new layer, called **adapter**, in between existing layers. Only these layers are trained, while the others are "frozen".

An adaptive layer is characterized by a simple fully-connected model and it looks like this:

![[Pasted image 20241028103918.png]]

The adapters are “injected” in a pre-trained (already working) model. If initialized randomly, it will “ruin” the intermediate results. To avoid this, initially, the adapter should behave as an identity function (adapter(x) = x). Then, during ﬁne-tuning, the layer adapts the  intermediate results. To obtain the "identity function" behavior we could use **residual** **connections**.

Results shows that we can achieve fine-tuned-similar performance at a fraction of the cost:

![[Pasted image 20241028104357.png]]

### Low Ranking Adaptation (LoRA)

During fine-tuning of a model we produces an incremental change $\Delta W$ applied to the weights. So after fine-tuning, we have:
$$
W'=W + \Delta W
$$
The idea is to **learn only the weight changes** $\Delta W$, because we can freeze $W$.
Learning $\Delta W$ is not particularly useful because it has the same number of parameters as $W$.
We could make an assumptions: $\Delta W$ is **near low-rank**. If this is true, we can approximately factorized $\Delta W$ into two smaller matrix: $A,B$.
If $\Delta W$ is near low-rank, we could **learn** $A,B$ such as that the product will be approximately $\Delta W$.
If we have $r > \frac{n}{2}$ then $A,B$ collectively have fewer parameters than $\Delta W$:
$$
W' = W + AB^T
$$
Authors show, empirically, that the updates are indeed near low-rank.
Considerations on LoRA:
- The beneﬁts increase as we increase the model size
- We can fine-tune the model on **multiple tasks**: $A_{t}, B_{t}$: we can store a **single instance of the original model**, then a pair of matrices for each task

### Prompt tuning

**Prompting**: approach of adding extra information for the model to condition on during its generation of the output. Normally, prompting is done by pre-pending some tokens to
the input, attempting to maximize the likelihood of the correct output.

Prompt tuning **adds new tokens** (task specific) that will be **fine-tuned** for improving performance. That is different from model tuning, because we only tune some tokens

![[Pasted image 20241028111652.png]]

### Quantization

Quantization is the process of mapping continuous (ﬂoating-point) values to discrete ones. In other words, reduce the prediction of the model’s weights and/or activation by limiting the range of allowed values.
This technique **does not go** under PEFT.
Pros:
- Reduces storage needed for the model
- Improves computational efficiency
Cons:
- It directly affects model performance

We need a **mapping scheme**: it specifies how values are mapped from a starting domain to another one. There are based on a **scale** and **zero-point**.

An example of mapping could be **absmax**: it scales values symmetrically (first image)
Another one is **zero-point quantization** (second one)

![[Pasted image 20241028112531.png]]

We can categorized quantization into two families, based on when we apply it:
- **Post-Training Quantization** (PTQ): the model is trained with a "normal" approach, then the weights are quantized afterwards. This could be easy but can lead to sub-optimal results
- **Quantization-Aware Training** (QAT): the model is aware of quantization, so in the forward pass we use (fake) quantized version of weights, in the backward pass quantized parameters are learned. Overall it provides better results, but we need to modify the training process

Last distinction on quantization:
- **Static** quantization: pre-compute scale, zero-point, then used them in a ﬁxed way. A validation set is used to measure distributions (calibration phase). Faster, more consistent
- **Dynamic** quantization: we compute scale, zero-point of each activation separately, this leads to a better use of range of possible values. We can avoid calibration, but it's computationally expensive

### LMM.int8()

This paper presented a technique to perform quantization called **"Vector-wise quantization"**: instead of computing a single scaling constant for a matrix, we compute it for each row/column of a matrix. Since **outliers** can appear into the matrices, we can decompose each matrix into non-outliers (8 bit precision) and outliers (16 bit precision).

![[Pasted image 20241030115826.png]]

The paper shows that if we don't use an appropriate quantization, the outliers will be to non optimal results.

![[Pasted image 20241030115943.png]]

A less dramatic options, is converting a model to **half precision (16 bit)**. By simply converting a model to half precision, we can save half the space of the model!
We preserve values in a continuous range of values, so this is not considered quantization
Also that is rather easy in PyTorch!

### Model distillation

A **smaller model can be distilled** from a larger one, so as to obtain behaviors similar to those of the original model.
This process is typically done with a student/teacher paradigm: the student is trained to **predict the teacher probability distribution** across all words.
By learning from the teacher, the student receives information unavailable in the ground
truth.

It’s been shown that distilled models can achieve comparable performance to the original ones, despite smaller architectures.
- DistilBERT
- TinyBERT
- MiniLM
- DistilGPT-2


