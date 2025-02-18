# Core concepts of Deep Learning

The **perceptron** is the simplest unit of a neural networks: it takes an **input** with **multiple features** and does three things: 
- It weights each features with a given **weight**
- It produces a weighted **sum** of the input
- It applies a **non-linear function** (**activation**)

![[Pasted image 20240923110625.png]]

We can **tune** our weights to represent a family of functions that a perceptron is representing.

### Activation functions

Activation functions are use for two main reasons:
- Enforce **properties** on perceptron's output
- Introduces **non-linearity** in the model

There are a bunch of "famous" activation functions that are employed: relu, sigmoid, softmax.

"Enforce properties on perceptron output" means that we can map the output of a linear combination of samples features to a numerical range $[0,1]$ in order to obtain a probability.
We can add **more perceptron** in parallel to creare a **layer**, and we can also creare new layers to create a pipeline: the input of a layer is the output of the previous layer.

"Introduces non-linearity in the model": if we don't add the non-linearity, it is useless to creare multiple layer, because we could simply create a different weight matrix.
In other words, if a network is linear, we can simply collapse it into one layer and obtain a linear function.
Non-linearity allows us to creare more complex functions (in classification problem we get more complex separation surface)

### Multi layer perceptron architecture

We can stack different layers separated by non-linearity.
The **Universal approximation theory** tells us that we can take any continuous function $g$ and there always exists a neural network with a single hidden layer and a finite number of neurons that can approximate $g$ with an arbitrary degree of accuracy.
The theory does not tell us how we can do it, nor the number of neurons needed.

![[Pasted image 20240923110716.png]]
### Activation functions for classification

In classification problem, the **output before the final activation** is treated as an **unnormalized probabilities** (**logits**, called $z_{i}$): we still need to convert logits into a valid probability.
We can use a **sigmoid** function to convert the logit $z$ to a numerical value in range $[0,1]$
$$
\sigma(z) = \frac{1}{1+e^{-z}}
$$
This guarantees that $P(pos|x) \in [0,1]$, and we can retrieve the negative class probability.

In **multiclass classification problem** we have a single logit for each possible class $c_{i}$. In this context we can apply the **softmax activation function**:
$$
P(c_{i}|x) = \frac{e^{z_{i}}}{\sum_{j}e^{zj}}
$$
In regression models, we typically need to predict real numbers: in those cases we can use a linear function as activation function $f(x) = x$

### Gradient Descent

We still need to figure out how to choose weights and biases (called $\theta$)
We can use an **objective function** (loss function) $\mathcal{L}(\theta)$ that we want to **minimize**: for example in linear regression we need to find an optimal $\theta$ to minimize the MSE (mean squared error).
To do so, we can differentiate the loss function with respect to the parameter $\theta$ then find the minimum of $\mathcal{L}$. If we are unable to differentiate, we can simply choose a bunch of $\theta$ and see which one "performs better".

This second approach can be used to choose an optimal $\theta$ of **more complex models**: we "move around" according to some "policy".
The "policy" we can choose is to move (with small steps) in the direction we the function decreases locally, i.e. in the **opposite** direction of the **gradient**:
$$
\theta_{t+1} = \theta_{t} - \alpha \cdot \nabla_{\theta}\mathcal{L}(\theta)
$$
$\alpha$ is known as **learning rate**, it controls the "size" of a step.
This approach is known as **gradient descent (GD)**.
One big **limitation** of GD is **sensibility to weights initialization**: different initial guesses may lead to different solutions, also GD can get stuck in a local minimum.

### Backpropagation

So far, we have assumed we are able to compute the gradient.
**Backpropagation** uses the chain rule and apply it to the end of the **computational graph**.

### Loss functions

In the context of Regression we use **MSE or MAE** (mean squared error vs mean absolute error).
For binary classification we can use **binary cross-entropy**: let's calls ground truth the real class labels $y=\{0,1\}$ while the predicted ones are $\hat{y} \in [0,1]$, the loss function is
$$
\mathcal{L} = -y\log(\hat{y}) - (1-y)\log(1-\hat{y})
$$
$y$ acts as a selector: we can choose which loss term we need to consider.
For the general case (multi class problem) we have a generalized loss function:
$$
\mathcal{L} = - \sum_{i} y_{i}\log(\hat{y_{i}})
$$

