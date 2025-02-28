# Introduction

Despite its name, the logistic regression is **discriminative** approach because rather than modelling the distribution of observed samples, we **directly modelling the class posterior distribution** $C | X$. Our goal will be to look for a model to predict $P(C=c | X=x)$.

We have seen that tied covariance MVG model gives these results:

![[Pasted image 20240518102711.png]]

Given $w$ and $b$ we can compute the posterior class probability:
$$
P(C=h_{1}| x,w,b) = e^{w^Tx+b} P(C=h_{0}| x,w,b) = e^{w^Tx+b}(1-P(C=h_{1}|x,w,b))
$$
Solving for $h_{1}$ probability term we obtain:
$$
P(C=h_{1}|x,w,b) = \frac{1}{1 + e^{-(w^Tx+b)}} = \sigma(w^Tx+b)
$$
Where $\sigma(x)$ is the **sigmoid function**:

![[Pasted image 20240518103156.png]]

# Model

Now we have obtained the following model that allow us to compute the posterior probability for class $h_{1}$ and $h_{0}$:
$$
P(C=h_{1}|x,w,b) = \sigma(x,w,b)
$$
This model assumes that decision rules are **linear surfaces** orthogonal to $w$. The parameters describing the model are $(w,b)$.
If we knew these parameters we could proceed, but since we don't know them yet, we will use a **frequentist approach** (i.e. compute estimates from training samples).

We firstly assume we have a labeled dataset with $n$ samples, and we also assume **independence** between samples.
Our goal is estimate $w$ with a value that maximizes the likelihood of our training samples.
$$
\text{let} \, \, \, y_{i} = P (C_{i}=1 | x,w,b) = \sigma(w^Tx_{i}+b)  
$$
It follows that:
$$
P(C_{i}=0 | x,w,b) = 1- y_{i} = \sigma(-w^Tx_{i}-b)
$$
We also notice that $y_{i}$ is the parameter of a **Bernoulli** distribution: $C_{i}|x,w,b \sim \text{Ber}(y_{i})$ 
So the likelihood function is:
$$
\mathcal{L}(w,b) = \prod_{i} y_{i}^{c_{i}}(1-y_{i})^{1-c_{i}}
$$
It's more convenient to work with logarithm so we want to **maximize** this log-likelihood function:
$$
l(w,b) = \sum_{i=1}^n[c_{i}\log y_{i} + (1-c_{i})\log(1-y_{i})]
$$
searching for $(w^*,b^*)$: the ML solution is also the solution that minimizes the average **cross-entropy** between the distribution of observed and predicted labels. So we can **minimize**:
$$
J(w,b) = - l (w,b) = \sum_{i=1}^n -[c_{i}\log y_{i} + (1-c_{i})\log(1-y_{i})]
$$
In particular the quantity inside the sum is called binary **cross-entropy** ($H(c_{i},y_{i})$) between the distribution of observed and predicted labels for the i-th sample.

Logistic regression looks for the minimizer of the average cross-entropy between the distributions for the training set labels of an evaluator $\epsilon$ who knows the real label and the distributions for the training set labels as predicted by the model $R(w,b)$ itself
The cross-entropy is a measure of goodness of the predictions, and the evaluation is performed over the training data itself.

We can rewrite the cross-entropy:
$$
H(c_{i},y_{i}) = \log(1+e^{z_{i}(w^Tx_{i}+b)}) \quad \quad z_{i} = \begin{cases}
1 \, \text{if } c_{i} = 1 \\
-1 \, \text{if } c_{i} =0
\end{cases}
$$
So the objective function is:
$$
J(w,b) = \sum H(c_{i},y_{i}) = \dots = \sum_{i=1}^n l(z_{i}(w^Tx_{i} +b))
$$
Where $l(x)$ is called **logistic loss function**:
$$
l(x) = \log(1+e^{-x})
$$

### Risk

Each time we make a prediction we pay a cost which is $l(z_{i},s_{i})$
Since $s_{i} = w^Tx_{i} +b$ is related to the distance of $x_{i}$ from the separating surface then we have two scenarios:
- We make a prediction and it agrees with the actual label: we pay a lower cost that  becomes exponentially smaller (asymptotically) as the absolute value of $s_{i}$ increases (we move away from the separation surface)
- We make a prediction and it disagrees with actual label: we pay a cost that increases (asymptotically) linearly with $s_{i}$

We can consider the logistic regression as a measure of **empirical risk** which must be minimized: more in general, the model goal is to minimize the risk over the training data.
We define the risk:
$$
R(\theta) = \sum_{i} l(w,x_{i},z_{i})
$$
A numerical solver iteratively looks for the minimizer of this function: We will use the **L-BFGS algorithm**.
The algorithm requires a function that computes the loss and its gradient with respect to $w$ and $b$.

### Regularization

If classes are **linearly separable** then the logistic regression is not well defined: this because there exists a $w$ and $b$ which allow samples to lie on the correct side of decision surface:
To solve this, we introduce a **penalty** term that avoids the norm $||w||$ to increase with no end.
This terms is described by the **regularization term** $\lambda$: so the function to minimize is:
$$
R(w,b) = \frac{\lambda}{2} ||w ||  + \frac{1}{n} \sum_{i=1}^n \log(1+ e^{-z_{i}(w^Tx_{i}+b)})
$$
Since $\lambda$ is a **hyperparameter** it cannot be computed by minimizing $R$ with respect to $\lambda$ (we will obtain $\lambda=0$): The selection of good values for $\lambda$ should thus be based on other approaches, such as cross-validation.
Regularization allows **reducing the risk of over-fitting** the training data.
Of course, if $\lambda$ is too large, we will obtain a solution that has small norm, but is not able to well separate the classes.
On the other hand, if $\lambda$ is too small, we will get a solution that has good separation on the training set, but may have poor classification accuracy for unseen data (i.e. poor generalization).

![[Pasted image 20240524175410.png]]

### Considerations

1. **Invariance to Linear Transformations**
   - The non-regularized logistic regression model is invariant to linear transformations of the feature vectors. This means that scaling or rotating the feature vectors will not affect the outcome of the model.
   - The regularized version of the model, on the other hand, is not invariant to these transformations. This is because the regularization term depends on the magnitude of the weights, which can be affected by changes in the scale of the features.

2. **Pre-processing of Data**
   - It is therefore useful, in some cases, to pre-process data so that the dynamic ranges of different features are similar. This can involve standardization (subtracting the mean and dividing by the standard deviation) or normalization (scaling the features to a range, e.g., \[0, 1\]).
   - Pre-processing helps in ensuring that all features contribute equally to the regularization term, preventing features with larger scales from dominating the model.

3. **Cross-validation**
   - Cross-validation can help in identifying good pre-processing strategies. By evaluating the model performance on different pre-processing techniques, one can choose the method that leads to the best generalization performance on unseen data.
