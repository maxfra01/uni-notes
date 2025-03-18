# 6. Recurrent Neural Networks and LSTMs architectures
## 6.1 Recurrent Neural Networks (RNNs)

In this context, we are dealing with **sequential data** such as time series, speech or music, or user's behaviors. However we could also apply sequential approach to non-sequence data: 
- For example we can classify images by taking a series of "glimpses"
- Or we could generate images one piece at a time
- Other stuff

![[Pasted image 20241108114430.png]]

The **key idea of RNN** is to have a "**internal state**" that is updated as a sequence is processed. We can process a vector $x$ applying a recurrent formula every time:
$$
h_{t} = f_{W}(h_{t-1}x_{t})
$$
Where $h_{t}$ is the state at time $t$, and $h_{t-1}$ is the previous state. Note that the function applied is always the same: $W$ does not change.
In the "vanilla" network the function $f_{W}$ is composed by a linear part, depending on $W$ and a non-linear part, made with a $\tanh$ function.
$$
h_{t} = \tanh(W_{hh} \cdot h_{t-1} + W_{xh} \cdot x_{t})
$$
Unrolling a recurrent neural network we get a similar architecture:

![[Pasted image 20241108114701.png]]

As said before, $W$ is always the **same**: the goal of RNN is to **learn the $W$ matrix**. In the example above we have a **many-to-many** network where each stage produces an output.
For each output we calculate a loss term $L_{i}$ and then we can combine all terms to produce a general loss.

We could also have a **encoder decoder** architecture, where the first part encodes the input sequence in a single vector, while the second produces an output sequence from a single input vector. The final loss calculated will influence both parts.

![[Pasted image 20241108115123.png]]

### Character-level Language Model

Let's consider for this task a **vocabulary** of chars. We could use a **one hot encoding** for the input chars. The task is to generate the next character of a word.

![[Pasted image 20241108115427.png]]
Notice that we are using one hot encoding, however this is trivial. In real implementations we could use an **embedding layer** which is more practical and skips the matrix multiplication.

### Loss

Calculations of gradients back in time is computationally expensive and also complicated: to avoid those problems we could use **truncated back propagation**: Run forward and backward
through chunks of the sequence instead of whole sequence.

![[Pasted image 20241108120329.png]]

### Image captioning

We could **combine different type** of neural networks. For example let's consider the image captioning task. We could use a **convolutional** NN as a **feature extractor** (an AlexNet for example), then we can remove the last layer from the CNN and use the output of the very last layer as input features for a Recurrent NN to predict the caption.

![[Pasted image 20241108121837.png]]

A popular dataset for image captioning is **COCO**: a large scale object detection, segmentation, and captioning dataset.

### RNN flow

The forward pass is quite straightforward, while the backward pass is more complex. For example we need to invert a matrix $W$. Besides this, we have a lots of blocks that needs to performs the same operations. 

![[Pasted image 20241108123253.png]]

If we need to multiply a singular value $> 1$, then the value will explode; on the other hand, if a value is $< 1$, then it will vanish. To mitigate the first case we could just perform **gradient clipping** (scale gradient if it is too big). To mitigate the second case we need to change architecture.

## 6.2 Long Short Term Memory (LSTM)

The main idea is: rather than having a single state $h_{t}$, we also have $c_{t}$ which is another "cell" state.
We also introduce new non-linearity with sigmoids.

![[Pasted image 20241108123535.png]]

The $i$ operation just tells us how much we should count on the $g$ output. The forget gate $f$ learns how much i want to maintain the knowledge of the previous $c$ state.
The output gate $o$ tells how much we want to reveal a cell, as next for the new input state.

The main advantage is the gradient, computed without the matrix $W$ multiplication. The issue discussed before is avoided thanks to LSTM architecture. In practice there is an uninterrupted gradient flow.

![[Pasted image 20241108124029.png]]

### RNN different architectures

Of course there is a **single layer** architecture, but we could also stack multiple layers on top of each other: we call this **multiple layer architecture**.
Other architecture are possibile. Even in this context it is possible to apply **Neural architecture search**, but studies show the performance are similar to LSTM networks.

### Recap

![[Pasted image 20241108124349.png]]
