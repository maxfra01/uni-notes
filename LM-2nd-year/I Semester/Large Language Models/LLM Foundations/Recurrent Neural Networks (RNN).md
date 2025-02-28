# Recurrent Neural Networks

Limits of fully-connected neural networks:
- Each **input** is expected to be **fixed in size**: for structured data (tabular ecc...) this is generally fine, but for unstructured data this is more troubling (for example images have different quality or ratio; texts have different lengths; audios too)
- Each input feature is associated to a weight: let's consider a FCNN that needs to predict if there is a dog in a picture: with fully-connected architecture the networks need to see a dog in all possible position of an image. This is inefficient and more data-intensive.
- The **output** is expected to be **fixed in size**: sometime we need non-fixed size output.

**Recurrent neural networks** are a type of NN designed to process **sequential data**: can receive a sequence as input and produce a sequence as output (input and output can be of varying size).
The idea is to **process one element at a time** (an element is the "unit" of the input sequence), while maintaining an **internal hidden state**, that summarize the information seen so far.

RNN takes two input: the first one is the element of the input sequence at the **current time step**; the second one is the **state produced at the previous** time step.
RNN produces two output: the prediction for the input element at the current time step, the state that will be used for the next time step.
To better visualize a RNN we can **unfold** the time steps:

![[Pasted image 20240930103312.png]]

Inside a RNN we typically find the following layer:
- Input layer: processes the current time input
- State layer: processes the state from the previous time step
- Output layer: processes the **combination of both** input and state layer (with a non-linearity) to produce the output.

![[Pasted image 20240930104724.png]]

How can we apply back-propagation through time? (BPTT)
A RNN is nothing other than a bunch of NN computed in cascade: we can just unfold all iterations of the RNN. The losses are made of multiple gradients terms that we simply add them up. Although the model is applied multiple times, the weights are the same.

![[Pasted image 20240930105222.png]]

Problems with RNN:
1. Gradients can be close to zero or to infinite, due to how they are computed
2. The hidden state should keep track of the history of the sequence, but in practice it mostly **remembers the most recent inputs**.
3. RNN **cannot be parallelized**, since the execution of each step depends on the previous one

### Vanishing of exploding gradients

If gradients are smaller than 1, then the final gradient will be close to zero: $0.8^{50}$
If gradients are greater than 1, then the final gradient will be a large values: $2.3^{50}$

### Long-term dependency issues

For long sequences, earlier steps are forgotten from the hidden state, in favor of the more recent ones:
- We can devise a “next word prediction” task
- At step t, the model predicts what the t+1th word should be
- “The mouse was getting chased by the cat through the big house. After a long chase, the cat finally managed to catch the ... ”
- The answer is (of course) mouse
-  However, RNN struggle with this kind of task, since they “forget” what they saw at earlier stages.

# Gated RNN

**Gated** RNN are a class of RNN designed to **address the long-term dependency and gradient issues of RNN**.
Two main families:
- **Long Short-term memory (LSTM)**: they manage the memory more effectively introducing gates
- **Gated recurring Unit (GRU)**: simplified version of LSTM with fewer gates but similar effectiveness

Key part of gated RNN:
- Input Gate: determine how much of the inputs should influence the current memory
- Forgot gate: decide which part of the memory should be forgotten
- Output gate: controls how much of the current memory should pass to the next time or should be output.
- Update gate (only in GRU): combination on input and forgot gate to make a simpler architecture

![[Pasted image 20240930110711.png]]

### Encoder-decoder architecture

The goal of a sequence2sequence is to perform a mapping from an input sequence to an output one.
Limitations of this task:
- Often, input sequence length is different from the output's length. For example consider the translation of a sentence in different languages.
- Sometimes we need to predict something that depends on the future
We need a new architecture: Encoder-decoder.
The encoder analyze the entire input sequence and produces a **state** (summarizing the entire input), then the decoder actually performs predictions.

### Limitations of gated RNN

Gated RNN mitigates the problem of classic RNN, but they are still present. Unfortunately Gated RNN still can't be parallelized.
