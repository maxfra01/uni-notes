# Introduction to ML

**Pattern** **Recognition**:
>Automatic discovery of regularities in data through the use of computer algorithms (...) to take actions such as classifying the data into different categories

**Machine** **Learning**:
>A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E^2.

Define models that are able to capture the regularities in our data and allow performing inference about the properties we are interested in. The models should not simply specify a set of human–defined rules, but should be able to learn from data. The learning stage should leverage observed data to improve the quality of the inference.

Applications of ML models are: classification, linear regression, density estimation.

We can identify two branches:
- **Supervised learning**: a model is fed with both input and output data (common in classification and regression): we need to estimate a mapping between input and output data
- **Unsupervised learning**: No feedback is provided to the system (common in clustering, density est.): the goal is to identify some useful structures of data.

Sometimes this methods can be used together: for example we can use unsupervised learning for pre processing steps, then we can use supervised learning.
We will focus on **classification** and **density estimation** (the goal is the estimation of distribution of input data).

# Pattern classification

The goal is to **assign a pattern to a class**: classes represent characteristics of the objects that we are considering 
We can identify two sub-category:
- **binary** classification: common applications are identity verification or intrusion detection
- **multiclass** classification: common applications are speech recognition or objcet categorization
We can also define:
- **closed set classification**: the outcome of the classification will be a possible value in a well-known set ($l_{1},l_{2}\dots l_{L}$)
- **open-set classification**: the outcome can be one value in a well-known set **and also none of them**

The process of **classification** can be divided into steps:

![[Pasted image 20240314171147.png]]

### Feature extraction

The goal of this phase is to **represent** an object in terms of numerical attributes, usually
arranged as vectors or matrices. 
Often it involves complex manipulations to extract a useful representation.

### Dimensionality reduction

The feature space is often very large and contains a large amount of unwanted and potentially harmful information.
Dimensionality reduction techniques compute a mapping from the n–dimensional feature space to a m–dimensional space, with m ≪ n.
Benefits from this phase are:
- Compressed information
- Unwanted data removed (noise)
- It helps data visualization
- It helps classification

In particular, this phase reduces **curse of dimensionality** and **overfitting**:

**Overfitting**:
>An over-complex model fits very accurately the observed data (very small training error), but is not able to provide accurate predictions for unseen data.

**Curse of dimensionality**:
>Volumes in high-dimensional spaces grow very fast, and data becomes very sparse

![[Pasted image 20240316160823.png]]

### Classification 

Given a feature vector representing an object, we need to associate a label to the object based on the properties of the representation.
The goal is basically a mapping operation from the m-dimensional feature space to theh space of labels.
The mapping is called **decision function**:
- **Discriminant model**: it simply takes as input the feature vector and the output will be a vector of label.
- **Discriminant non-probabilistic model**: it takes as input the feature vector and maps each object into a set of scores.

- Discriminative probabilistic model:  the model computes $P(C_{k} | x)$ and assigns a label according to **posterior probabilities**.
- Generative probabilisti model: The model describes the data generation process in terms of class-condition distributions $P(x | C_{k})$. The model can incorporate **application-dependent** prior class information $P(C_{k})$. Class assignment is based on highest posterior probability: $$
P(C_{k}|x) = \frac{P(x|C_{k}) \cdot P(C_{k})}{P(x)}
$$
### Solving the inference problem

Considera a model $M$ describing relationship between features and labels.
There are two families of models:
- **Non parametric** model: the complexity grows with sample size
- **Parametric** model: the complexity depends on a set o parameters $\theta$ (whose size does not depend on the data).
Now consider a training data set (labeled) $D$: we can define a **frequentist** (non-probabilistic) model where we estimate the parameters $\theta$ of the model:
$$
D,M \to \theta^*
$$
Another way to set parameters values is using the **Bayesian probabilistic approach**: update the prior belief over the model parameters using the observed data
$$
D,M, P(\theta, M) \to P(\theta | D,M)
$$
After learning the model parameters we need to predict the class label for a test sample: this is equivalent to
$$
P(x_{t}|C_{k},D,M) \approx P(x_{t}| C_{k}, \theta^*, M)
$$
for the frequentist approach, while for the Bayesian approach we need to compute a integral:
$$
P(x_{t}| C_{k}, D) = \int P(x_{t}|C_{k}, \theta, M) \cdot P(\theta | D,M)  \, d\theta 
$$
### Model evaluation

After making a prediction we need to be able to assess to quality of it: we are interested in performance on **unseen data**.
In order to do that we use a **test set** (labeled data): this test set should mimic real use cases.

Models and training procedures often contain **hyperparameters** (Number of dimensions of reduced features, Step size in gradient-descent based optimization, Regularization coefficients), but the optimal values cannot be estimated using the same method of model parameter: maybe we can have different competing models, and we need to select the one that provides the best predictions (**model selection**).
To perform model selection we cannot use test data, so we introduce a new set known as **validation set**: we can build validation data by extracting some data from training set.

If training data is scarse, we can use **$K$-fold cross validation**:
We split data into $K$ different folds, then we use $K-1$ folds to train the model, while the remaining fold is used as validation data. Then we iterate the process for all folds.

![[Pasted image 20240321155526.png]]

- Using large values of $K$ requires more computational power, but we train the model with lots of information
- Using small values of $K$ requires less computational resources, but the model is trained with less data
- Particular case: **leave-one-out** (we use all samples as training data, and just one sample for validation)

Next: [[Dimensionality reduction]]