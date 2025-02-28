# Gaussian classifiers

Let's suppone we have a classification problem: we need to identify the gender of a suspect based on his height:
A simple approach to identify the gender based on a given height is to count the number of sample with that specific value:
$$
P(M| 174cm) \quad \quad P(F | 174cm)
$$
Following Bayes rules we can rewrite these two expressions:
$$
P(C_{t}=c | X_{t} = x_{t}) = \frac{f_{X,C}(x_{t},c)}{\sum_{c' \in C} f_{X,C}(x_{t},c')}
$$
Which it's called **posterior probability**.

Intuitively, we can fit a Gaussian density over the samples of each class: now we look for a method to predict the gender of a 174cm tall suspect.

![[Pasted image 20240418151956.png]]

We can compute mean and variance of the distributions, and also compute the likelihood function for each class. Since loglike of  MALE is 4.5 times greater than FEMALE, it means that is 4.5 times more likely to observe the given height in a male, however this is not sufficient to discriminate.
We also need to compute the posterior probability:

$$
P(C=M | 174cm) = \frac{f_{X,C}(174 | M) \,\,P(C=M)}{f_{X}(174)} \quad \quad  P(C=F | 174cm) = \frac{f_{X,C}(174 | F) \,\,P(C=M)}{f_{X}(174)}
$$

Since we need to compute the ratio, it is useless to compute $f_{X}(174)$.
The ratio would be:
$$
\frac{P(C=M|174) \, \,P(C=M)}{P(C=F|174) \, \, P(C=F)} 
$$
We need to assume that in the dataset males and females are equally distributed, so $P(F) = P(M) = \frac{1}{2}$. It's also possibile that my dataset has a greater number of female samples (for example $P(C=F) = 0.9$ ).

Now we will **formalized** the method just used above.

We assume that our data, given the class, can be described by a gaussian distribution: each class will have a **class mean** and a **class covariance matrix**:
$$
f_{X|C}(x_{t}|C) = \mathcal{N}(x_{t}| \mu_{t}, \Sigma_{t})
$$
Unfortunately we don't know the correct parameters, so we will compute a maximum likelihood estimate. 
We need also to assume that alla observation are i.i.d and training and evaluation sets are independent.
For univariate R.V.s, we have already shown that the ML solution corresponds to the class mean and covariance matrix.

### Binary classification

For a binary problem with two classes: $h_{1}, h_{0}$
We can express a threshold using the ratio between posterior probability:
$$
r(x_{t}) = \frac{P(C=h_{1}| x_{t})}{P(C=h_{0}|x_{t})}
$$
We can also express this in terms of logarithm:
$$
\log r(x_{t}) = \log \frac{f_{X|C}(x_{t}|h_{1})}{f_{X|C}(x_{t}|h_{0})} + \log \frac{P(C=h_{1})}{P(C=h_{0})}
$$
The first element of this sum is called **log-likelihood ratio** $llr(x_{t})$, while the second term corresponds to the prior probability.
Since we are working with logs, the new **decision is based on the following comparison**:
$$
\log r(x_{t}) \gtrless  0
$$
The log-likelihood ratio acts as a **score**, with a probabilistic interpretation Greater scores values imply our system favors class $h_{1}$, lower values mean it favors class $h_{0}$.
The decision requires comparing the LLR to a threshold $t$ that depends on the application, through the class prior probability $\pi$.
$$
 \log r(x_{t}) = \log \frac{f_{X|C}(x_{t}|h_{1})}{f_{X|C}(x_{t}|h_{0})}  \gtrless -\log \frac{\pi}{1-\pi}
$$
The decision function is **quadratic** in $x$, so the Gaussian classifier has a **corresponding surface**, where each of the threshold is associated with a line that splits the decisions:

![[Pasted image 20240419131025.png]]

If we have a multiclass classification problem we can compute the posterior probability for all classes.

To summarize, the two assumptions we need to do to use this model are:
- Data can (probably) come from a gaussian distribution
- We have samples to compute mean and covariance matrix

### Naive Bayes classifier

If samples are few, compared to their dimensionality, the estimates can be inaccurate.
This issue is more evident with covariance matrixes.
In this cases we can suppose that different components are approximately independent, so we can simplify the distribution of $X|C$:
$$
 f_{X|C}(x|c) = \prod_{j=1}^D f_{X_{[j]}|C}(x_{[j]}|c)
$$
where $x_{[j]}$ is the $j$-th component of a sample (not to be confused with $x_{j}$ which is the $j$-th sample of the dataset): this means that each attribute has its own distribution.
The class distribution is simply the product of the densities of alla attributes.

We can assume that each feature has a univariate gaussian distribution: if we do that we can use likelihood to estimate parameters:
$$
\mu_{c,[j]}^* = \frac{1}{N_{c}} \sum_{i|c_{i}=c} x_{i, [j]}

\quad \quad \quad

\sigma^2_{c,[j]} = \frac{1}{N_{c}} \sum_{i|c_{i}=c} ( x_{i,[j]}- \mu_{c,[j]})^2
$$
Then for a generic sample, since we need to combine all attributes, we write:
$$
f_{X|C}(x|c) = \mathcal{N}(x|\mu_{c}, \Sigma_{c})
$$
Note that Naive bayes classifier corresponds to a multivariate gaussian classifier with a **diagonal covariance matrix**.
Since we are working with a diagonal covariance matrix we have quadratic form:

![[Pasted image 20240419141836.png]]

### Tied Covariance Model

Another common Gaussian model assumes that the covariance matrices of the different classes are **tied**.
We can model the data as the sum of class mean and an independent gaussian term.
We also need to use **the same covariance matrix** for all classes:
$$
f_{X|C}(x|c) = \mathcal{N}(x| \mu_{c}, \Sigma)
$$
We can compute estimators for class mean (same as always) and the covariance matrix:
$$
\Sigma^* = \frac{1}{N} \sum_{c} \sum_{i|c_{i}=c} ( x_{i}- \mu_{c})(x_{i}-\mu_{c})^T
$$
This term is equal to what we called **scatter within** classes in PCA.
For the tied model we can computer the binary log-likelihood, but since the covariance is the same for all classes we can write:
$$
\text{llr}(x) = \log \frac{\mathcal{N}(x;\mu_{1},\Lambda^{-1})}{\mathcal{N}(x; \mu_{0},\Lambda^{-1})} = x^Tb + c
$$
with $b = \Lambda(\mu_{1}-\mu_{0})$ and $c = -\frac{1}{2}(\mu_{1}^T\Lambda \mu_{1} - \mu_{0}^T\Lambda \mu_{0})$ so the decision function in this case is **linear in x** 

![[Pasted image 20240419134806.png]]

The tied model is closely related to LDA:
Projecting samples in LDA space has the following results:$$
w^Tx = k \cdot x^T\Lambda(\mu_{1}-\mu_{0})
$$ which corresponds to the classification rule of gaussian model with tied covariance: indeed LDA assumes the all classes have same covariance matrix.
Here we can change threshold based of prior probability, with LDA you could not do that, because we had to choose an arbitrary threshold.

### Consideration

- If data is high-dimensional, PCA can simplify the estimation. PCA also allows removing dimensions with very small variance (e.g. in the MNIST dataset, pixels that are white for all images regardless of the digit)
- Multivariate models perform better if we have enough data to reliably estimate the covariance matrices
- Naive Bayes can simplify the estimation, but may perform poorly if data are highly correlated
- Tied covariance models can capture correlations, but may perform poor when classes have very different distributions — on the other hand, if we have reason to believe that covariances shouldbe very similar, then the model will provide a more reliable estimate