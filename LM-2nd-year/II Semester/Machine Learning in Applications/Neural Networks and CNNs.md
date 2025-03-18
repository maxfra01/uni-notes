# 6. Neural Networks and Convolutional Neural Networks

## 6.1 Neural Networks

Basic component: **perceptron**, which is a single component that acts as a weighted sum of inputs and weights, then a "step function" is applied.
If we call $w$ the weights vector and $x$ the inputs, we can describe the perceptron as:
$$
h(x_{i}) =\text{sign}(w^Tx_{i}+b)
$$
Perceptron can be used as classifier with the following algorithm:
1. Start with weights = 0
2. For each training instance we classify a sample
3. If we are correct we do nothing, otherwise we update the weights using this formula: $w=w+y^* \cdot x$
If data are linearly separable, then the perceptron will commit at most $\frac{1}{\gamma^2}$ mistakes, where $\gamma$ is the "margin"

![[Pasted image 20241015175613.png]]

Problems with perceptron:
- poor generalization
- overfitting
- If data are not linearly separable, then we get trash

To **improve the perceptron** we could add a non-linearity to provide more "probabilistic" output: for example, instead of the step function we could use a sigmoid.

### Deep ReLU network

Stacking ReLU function as the output of layers, we can approximate any complicated function, to model real world.

This concept above is generalized in the following theorem:

![[Pasted image 20241015182534.png]]

These type of theorem are not constructive, i.e. they do not tell us how to find these approximation, but they guarantee the existence.

### Training

In general we consider the MSE as the loss for the training process. However the loss in not convex w.r.t. the parameters $\Theta$. We can identify special cases:
- If we have one layer, no activation, MSE as loss -> Linear regression
- If we have one layer. sigmoid activation, logistic loss -> Logistic regression

In general we are dealing with a combination of linear and non-linear.
In order to optimize this model we need to compute gradients, but computationally that is non feasible. Even if we try to use finite differences (definition of derivative) it is not feasible. So we need another strategy, called **backpropagation**, which basically uses the chain rule.
Considering a function we try to compute a **computational graph**, which is basically an acyclic directed graph that describe the steps to compute a mathematical expression with all intermediate steps.

![[Pasted image 20241022161923.png]]

The evaluation of a function $f(x)$ corresponds to a forward traversal of the graph.

We can now compute the derivatives of all nodes based on the corresponding inputs.
Doing this way, the cost of computing the derivative of $f(x)$ is the same of computing $f(x)$ itself, because we are composing it with little pieces.

![[Pasted image 20241022162519.png]]

However we are dealing with multi-dimensional input functions $f: \mathcal{R}^p \to \mathcal{R}$
So the cost of computing the **gradient of $f(x)$** is equal to:
$$
\text{cost of } \nabla f(x) = p \times \text{cost of computing } f(x) 
$$
The approach described so far is known as **forward mode**, because we are traversing the graph and computing all partial derivatives w.r.t. the input $x$ (i.e. straightforward application of chain rule). This method accumulates numerical derivative evaluations rather than expression. 

However we need to change the mode of the problem, by computing alla derivatives of $f(x)$ w.r.t. to **inner nodes**: this is called **reverse mode**.

![[Pasted image 20241022163310.png]]

Since we need internal nodes values, we need to:
1. Make a **forward pass**, where we ONLY calculate the function values of internal nodes
2. **Backward pass** to compute derivatives

When dealing with loss function $l: \mathcal{R}^p \to \mathcal{R}$ where $p$ is the number of weights, we don't need to calculate simple derivatives, but instead we **need to calculate gradients and Jacobians**. 
It can be proved that:

![[Pasted image 20241022163801.png]]

So the reverse mode does not have that $p$ factor that would make the calculation non feasible.

This all logic is called **back-propagation**, which is "reverse mode automatic differentiation" applied to deep neural networks. With this approach, evaluation the loss $l$ is as fast as evaluating the gradient $\nabla l$.

Some observations on the loss function:
- Loss function is not convex in general, so we will be satisfied with a local minima.
- Searching for a global minimum generally tends to overfit the model
- In general there are multiple minima, and the one reached depends on **weight initialization**.
- In general, the loss is **non-differentiable**, however software returns one of the one-sided derivatives
- Numerical issues are always present

So training a network means looking for a minima for the loss, however there is no correct way of training a network.

Our approach will be **gradient descent**: looking at the **loss landscape** we iteratively move in the direction where the function decreases the most.

![[Pasted image 20241015163611.png]]

In other words we compute at each time step $t$:
$$
\Theta^{(t+1)} = \Theta^{(t)} - \alpha \cdot \nabla l_{\Theta (t)}
$$
This approach requires that **function is differentiable**.
Also, gradient descent **may remain stuck in stationary point**, but they are not local minima.
The $\alpha$ parameter is called **learning rate**: being able to select this parameter in an accurate way may lead to efficiency.
- If $\alpha$ is small we will have slow convergence
- If $\alpha$ is too big we will have overshooting
- Selecting the correct $\alpha$ allows us to reach the local minimum in a certain time

The learning rate can be **adaptive**:
1. We can decrease $\alpha$ to a **decay parameter** $\rho$
2. We can accumulate past gradients and keep moving in their direction ![[Pasted image 20241015165403.png]]With the first equation we are accumulating the gradient at each step (momentum), then with the second equation we are moving in a direction. The more the gradient is pointing to a certain direction, the faster we will move.

We can also apply gradient descent to non-convex problem, but there are no guarantee that we find a minimum. However, to gain generalization, we are rarely interested in a global optimum. (it means overfit the model)
We are more interested in local optimum, more efficient and computationally feasible.

### Stochastic gradient descend

There are limitations to classic gradient descend: the number of samples and the number of parameters can be very high. To avoid this problem we can use a **mini-batch** extracted from the training sample to approximate the gradient. Even if the mini-batch is small, it should be representative for our data.
After applying this approach, the gradient is approximated, but with a **significantly speedup**.

## 6.2 Convolutional Neural Networks (CNNs)

As described above, images (data) is often composed of hierarchical, local, shift-invariant patterns. A CNN **directly exploits this fact as a prior**.

![[Pasted image 20241022170510.png]]

We are applying the convolution operator into a **convolutional layer**: we talk about **weight sharing** because the outgoing edges have the same weights for each "window".

```ad-note
title: Convolution
Given two function $f,g: [-\pi,\pi] -> \mathcal{R}$, their convolution is a function:
$$
(f * g)(x) = \int_{-\pi}^{\pi} f(t) g(x-t) dt
$$
Convolution is a **commutative and shift-equivariant operator**.
```

On images, we can think the convolution as **applying a filter** on them.
On 2-D domains, with RGB $R^2 \to R^3$ then we can compute convolution as:
$$
(f*g)[m,n] = \sum_{k} \sum_{l} f[k,l]g[m-k,n-l]
$$
We can visualize the convolution as a **sliding windows** (square) moving on the image. 

![[Pasted image 20241022171459.png]]

The new parameters of this networks will be the value of the sliding window elements.

![[Pasted image 20241022171715.png]]

For simplicity imaging the filter as a 3 x 3 grid that is applied to all the image: in this case the weight of each cell will be applied to the image: so now the concept of **weight sharing** makes sense.
In first layer, a network is dealing with larger portions of input, so we do not have a useful view of the interactions in the image
The pooling stage is helpful because it allows to **capture non-local interaction** via simple building block that **only describe sparse interactions.**
Pooling can be exploited via many strategies: a simple one can be max pooling.

![[Pasted image 20241022172329.png]]

So, CNN exploits the following features:
- Convolutional filters (Translation equivariance)
- Multiple layers (Compositionality)
- Filters localized in space (locality)
- Weight sharing (Self-similarity)

Let's consider a 32 x 32 x 3 image as input of a convolutional layer, and a filter of size 5 x 5 x 3. We can **convolve** the filter with the image.

![[Pasted image 20241022175010.png]]

The effect of the filter is getting a new matrix of size 28 x 28, which is called **activation map**. Of course we can apply more than one filter to the original image. We can horizontally stack the activation maps to get a "new image" which will be fed to the new layer

![[Pasted image 20241022175338.png]]

A popular example: ConvNet is a sequence of Convolutional Layers, interspersed with
activation functions (example ReLU) .

In general, if we are working on a $N\times N$ image, with a $F \times F$ filter and a stride $S$ (how much we move the filter at each step), a convolutional layer will create an activation map of:
$$
\frac{N-F}{S} +1
$$
Of course, not all possible strides are allowed, however it is possible to apply a **zero-padding** to the input image, so we can choose any $S$ value.

Another concept we need to introduce is the **receptive field**: each element in the output depends on a $K \times K$ area in the input. With this notation, the filter is called **kernel** and its size is $K \times K$.

![[Pasted image 20241022180818.png]]

Note that a 1 x 1 filter makes perfect sense, because it is useful **to reduce output depth**.

```ad-example
title: Output volume size?
Input volume: 32x32x3
10 5x5 filters with stride 1, pad 2

Remember that, if we are working with padding we need to double-add it to the input size ($N=N+2*P$)

Output volume size is: (32+2*2-5)/1 +1 = 32 spatially, so 32x32x10

Number of parameters in this layer: each filter has 5x5x3 +1 = 76 parameters, so for 10 filters we have 10*76=760 parameters

Numbers of multiply-add operations?
10x32x32 outputs, each output is the inner product of two 5x5x3 tensor, for a total of 10*32*32*5*5*3= 768k
```

A quick recap:

![[Pasted image 20241022181621.png]]

The **pooling** makes the representation smaller and more manageable. The pooling function is an **hyperparameter** (so the model cannot learn it, such as the kernel size and the stride).
The pooling operates on each activation map independently.

![[Pasted image 20241022181851.png]]


### Dropout

First technique to perform regularization is to add a penalty term.
Another technique is **dropout**: in each forward pass, randomly set some neurons to zero. Probability of dropping is a **hyperparameter**, for example 0.5 is common.
This introduces some randomness into the network: it forces the network to have a **redundant representation**, it also **prevents co-adaptation** of features.

Dropout during test time, requires to multiply with its probability:

![[Pasted image 20241025124502.png]]


## 6.3 Common Architectures

### AlexNet

Some recall to calculate the parameter of a CNN:
- Output channels equals the number of filters
- Output size = $\left( \frac{W-K}{2} +1 \right)$
- Memory size: $CH'W' \cdot 4B$
- Number of Floating point operations: $C_{out}H'W' \cdot C_{in}KK$

**AlexNet** is composed from the following **layers**:

![[Pasted image 20241105175934.png]]

Note that the flatten layer unravel the multiple-channel input into a vector, that will be the input of the fully connected part of the layer.
Alex has **three fully connected layer** . The entire architecture was build via trial and error approach, and it took a very long time.
Looking at the measures of memory, parameters and FLOP we notice a trend: Most of the memory is used in the first layers. FLOP are mostly in the convolutional part, and the majority of the parameters are in the fully connected part.

![[Pasted image 20241105180702.png]]

AlexNet won a competition where a **top 5 error** metric was employed: a prediction is considered wrong if the correct class does not appear in the 5 highest probability output of the model.

### VGG

The VGG group came up with some **design rules**: 
- All convolutional layer are 3 x 3 stride 1 pad 1.
- All pooling are 2 x 2 stride 2
- After pool, the number of channels is doubled

Networks gain some regular design, characterized by blocks.

![[Pasted image 20241105181427.png]]

Why those 3 x 3 convolutional layers? Because two 3 x 3 convolutional layers have the same receptive field as a single 5 x 5 layer, however **they have less parameters and takes less computation**.

Compared to AlexNet, VGG is a much bigger network: 25x memory usage, 2.3x params, 19.4x FLOP

### GoogLeNet (InceptionNet)

The main idea is to have a **deeper network**, with computational **efficiency**.
The main component is called **inception module**: this module is able to perform multiple convolution with different kernel size in parallel.
Another component is the **aggressive stem**: at the start the network aggressively **downsamples the input**: this allows to have a lighter workload at the start.

Then we have the inception module:

![[Pasted image 20241105182542.png]]

At the end there is **only fully connected layer**: 

![[Pasted image 20241105182634.png]]
There are also the **auxiliary classifier**: at that time there was no batch normalization, to make the model converge they added those classifier in the middle of the network. The idea is to have an extra loss from the classifier that helps the model to converge (to be guided in the right direction). 

![[Pasted image 20241105182759.png]]

### Residual networks

In 2015 **ResNet was introduced**: it was a real revolution towards **depth** (152 layer for ResNet). Once Batch normalization was introduce the researchers tried to go deeper, but the deeper model performed poorly. The initial guess was overfitting but the behavior was strange.
The real reason behind this trend is **optimization**: Deeper model are **harder to optimize** and in particular they don't learn the **identity function** to emulate shallow models.

The solution to this is let the network know that **some layers are not needed**: we add what are called **skip connections** (shortcut).
Networks are now composed of **residual block**: the model itself need to learn if it has to use a certain **stage**.

![[Pasted image 20241105183747.png]]

ResNet also implements the idea from GoogLeNet of **aggressive stem** to reduce the initial workload. It also implement a **global average pooling and a fully connected layer**.

**Basic vs Bottleneck** residual block:

![[Pasted image 20241105184034.png]]

Nowadays **ResNet** is the standard and still widely used!
Model ensembles is widely used also.

### Other CNN Architectures

LeNet-5, introduced in 1998, was one of the earliest CNN architectures and set the foundation for modern deep learning models. It consists of two convolutional layers followed by fully connected layers and was primarily designed for handwritten digit recognition.

Xception, introduced in 2016, replaced traditional convolutions with depthwise separable convolutions, significantly reducing the number of parameters while maintaining performance. This approach splits the convolution process into depthwise and pointwise operations, increasing computational efficiency.

Inception-v4 and Inception-ResNet, both introduced in 2016, improved upon earlier Inception versions by incorporating more inception modules and integrating residual connections to enhance gradient flow and training efficiency. The use of residual connections in Inception-ResNet combines the benefits of both architectures, enabling deeper networks without vanishing gradient issues.

ResNeXt, introduced in 2017, built upon ResNet by introducing grouped convolutions, which allow for multiple parallel pathways within each block. This modification improves efficiency and accuracy while keeping the parameter count manageable.