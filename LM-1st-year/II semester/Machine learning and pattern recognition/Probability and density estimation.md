# Probability recap

### Multivariate Gaussian Distribution

We can extend Gaussian distribution to **random Vector**: let $X$ be a random vector $X = [X_{1},\dots, X_{N}]^T$ where $X_{i}$ are i.i.d. standard normal distributed R.V.s ($X_{i} \sim \mathcal{N}(0,1)$).
The jointly distribution of $X$ is given by the product of each distribution (or equivalently):
$$
f_{X}(x) = (2\pi)^{-N/2}e^{-(1/2)x^Tx}
$$
We can say that $X$ follows a **multivariate gaussian distribution** (MVG) with mean $\mu$ and covariance $\Sigma$ if $X$ can be written in the following way:
$$
X = AY + \mu
$$
where $Y \sim \mathcal{N}(0,I)$ and $\Sigma = A A^{T}$
The probability density function of $X$ is given by:
$$
f_{X}(x) = (2\pi)^{-N/2} |\Sigma|^{-1/2} \cdot \exp\left( -\frac{1}{2} (x-\mu)^T \Sigma^{-1} (x-\mu)\right)
$$
The term $\Sigma^{-1} = \Lambda$ is called **precision matrix**.

### Density estimation

Starting from an example, we would like to predict whether a coin flip will result in head ot tail.
Every toss can be considered as a single Random variables: our goal is to predict the probability that a new toss $X_{t}$ will result in head.
Suppose for a moment that we know the probability of head, which is $\pi$
We also assume that all tosses are i.i.d: $P(H) = \pi$
This means that $X_{t}$ will be Bernoulli distributed, but in real cases we do not know $\pi$

We have two estimation model:
- Bayesian
- Maximum likelihood

**Maximum likelihood** consists in **searching a "good" value of $\pi$ that best explains the observed tosses**:
$$
\mathcal{L}(\pi) = P(X_{1}=x_{1},\dots X_{n}=x_{n}| \pi)
$$
Now $\pi$ is considered an unknown value (not random).
Since tosses are i.i.d and each toss is Bernoulli distributed we can express likelihood as:
$$
\mathcal{L}(\pi) = \prod_{i=1}^n \pi^{x_{i}} ( 1-\pi)^{1-x_{i}}
$$
We can compute the **maximum likelihood estimator** by **maximizing the likelihood** function:
$$
\pi^*_{ML} = \arg \max_{\pi} \mathcal{L}(\pi) = \arg \max_{\pi}  \prod_{i=1}^n \pi^{x_{i}} ( 1-\pi)^{1-x_{i}}
$$
It's more easy to maximize the logarithm of likelihood function $l(\pi) = \log(\mathcal{L}(\pi))$


When we need to model continuous values, Gaussian distributions arise naturally in a wide variate of contexts. Let's assume we have some data $\mathcal{D}$.
Since each of the sample has the following density:
$$
f_{X | \theta} = \mathcal{N}(x| \mu, \nu)
$$
then the likelihood function is given by combining densities of all sample:
$$
\mathcal{L}(\theta) = \prod_{i=1}^{n} \mathcal{N} (x_{i}| \mu_{i}, \nu_{i})
$$
You do not know the "correct" values of $\mu , \theta$, so we need to estimate them: using the **method of moments (MOM)** we obtain:
$$
\mu^*_{MOM} = \frac{1}{N} \sum_{i} x_{i} \quad \quad \nu^*_{MOM} = \frac{1}{N} \sum_{i} (x_{i}- \mu^*_{MOM})^2
$$
