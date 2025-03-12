# 4. Classification Models

## 4.1 Generative vs. Discriminative Models

| Generative                                              | Discriminative                                                       |
| ------------------------------------------------------- | -------------------------------------------------------------------- |
| Represent both the data and the labels                  | Learn to directly predict the labels from the data                   |
| Often, makes use of conditional independence and priors | Often, assume a simple boundary in the feature space (e.g., linear)  |
| Models of data may apply to future prediction problems  | Often easier to predict a label from the data than to model the data |
| Example: Naive Bayes Classifier                         | Example: Support Vector Machines                                     |

## 4.2 Nearest Neighbor Classifier

Nearest Neighbor Classifier is a classification method that find the **nearest neighbor of the feature vector** to be classified and assign the class of the neighbor. It works by keeping all data into some efficient look-up structure. It can be simply be extended to a $K$-nearest neighbor.
The main benefit of KNN is the **simplicity** of the algorithm, however:
- KNN suffers from **curse of dimensionality**
- It's very sensitive to unbalanced data
- It's very sensitive to the choice of $K$

## 4.3 Bayesian Classification

Bayesian classification is a **probabilistic classification** method based on **Bayes’ Theorem**. It predicts the probability of a class given some observed data.

Bayes’ theorem describes how to update our prior beliefs based on new evidence:
$$
P(H | X) = \frac{P(X | H) * P(H)}{P(X)}
$$
Where:  
- $X$ = observed data (evidence)  
- $H$ = hypothesis (class label)  
- $P(H|X)$ = posterior probability (probability of class given evidence)  
- $P(H)$ = prior probability (initial probability of class)  
- $P(X|H)$ = likelihood (probability of observing data given the class)  
- $P(X)$ = evidence (probability of the data occurring)  

**Classification Goal:** Assign $x$ to class $C_{i}$ with the highest $P(C_{i}|X)$.  

### Naive Bayes Classifier 
A simplified version of Bayesian classification that **assumes conditional independence** between features:  
$$
P(X|C_{i}) = \prod_{k=1}^N P(x_{k}|C_i)
$$
This assumption **greatly reduces computation cost**.  
- If a feature is **categorical**, probabilities are computed from frequency counts.  
- If a feature is **continuous**, probabilities are modeled using a **Gaussian distribution**:

Avoiding the **Zero-Probability Problem**:  
If a feature $x_{k}$ never appears in training data for a certain class, its probability is **zero**, which leads to incorrect predictions.  
To fix this, we apply **Laplacian smoothing** (add 1 to all counts).  

| Pros of Naive Bayes            | Cons                                           |
| ------------------------------ | ---------------------------------------------- |
| Easy to implement, very fast   | Assumes feature independence (not always true) |
| Works well with small datasets | May lose accuracy if dependencies exist        |
| Good for categorical data      | Not always best for numerical data             |

Naïve Bayes is often used in **text classification, spam filtering, and medical diagnosis** due to its simplicity and effectiveness despite its strong independence assumptions.

## 4.4 Support Vector Machines (SVMs)

Support Vector Machines are non-probabilistic binary linear classifier. The goal of SVMs is to predict an hyperplane or a set of hyperplanes to separate data. They can be used both for classification or regression.

Considering a linear separation of data  (boundary) we  define the **margin** of a linear classifier as the width that the boundary could be increased by before hitting a datapoint.
The maximum margin linear classifier is the linear classifier able to separate the training data points with the maximum margin.
This is the simplest kind of SVM (Called **Linear** SVM).

![[Pasted image 20250305152314.png]]

This Implies that only **support vectors are important**; other training examples are ignorable.
In Linear SVM (**Hard margin SVM**) all datapoints should be classified correctly, i.e. there is no training error. If data are noisy or not separable we need to change strategy.

To mitigate noisy points we use soft margin SVM.
In **Soft Margin SVM** we allow some misclassification of datapoints by a coefficient $\xi_{i}$. This process also reduce the risk of overfitting.


If data is non-linear separable, then we should apply a **kernel** to map datapoints to an higher-dimensional space where there are separable:
$$
\Phi: x \to \phi(x)
$$
This approach is called **Non-linear SVMs**.

![[Pasted image 20250305152854.png]]

Similar to Kernel PCA we do not have to work in the higher dimensional space at training or testing time. It can be shown that during the training, the optimization problem only uses the training examples to compute pair-wise **dot products**.
There exist functions that, given two vectors, implicitly compute the dot product between them to an higher-dimensional space without explicitly transforming them.
Such functions are called **kernel** functions.

To summarize:
1. By using a kernel $K(x_{i},x_{j})$ we can implicitly transform datasets to a higher-dimensional space using no extra memory, and with a minimal effect on computation time.
2. We can efficiently learn nonlinear decision boundaries for SVMs simply by replacing all dot products$x_{i},x_{j}$ in the optimization problem with $K(x_{i},x_{j})$.

Unfortunately, choosing a 'correct' kernel function is a nontrivial task, and may depend on the specific task at hand: Polynomial kernel, Radial Basis Function (RBF) kernel, Sigmoid kernel, …
However it's very important to **tune the kernel hyperparameters**.

**Multi-class SVM** has no direct formulation. We need to combine multiple binary SVMs and we have two approach: one vs other, one vs one.

| Pros of SVMs                                                                | Cons of SVMs                                             |
| --------------------------------------------------------------------------- | -------------------------------------------------------- |
| Many publicly available SVM packages                                        | No “direct” multi-class SVM, must combine two-class SVMs |
| Kernel-based framework is very powerful, flexible                           | Computation, memory                                      |
| SVMs work very well in practice, even with very small training sample sizes | Tuning kernel's parameters might be tricky               |

## 4.5 Decision Trees

Decision trees are a classification technique where the representation is a tree with finite depth. In each **node** of the tree there is a test on a single **attribute** (i.e. a feature of data) and every **branch** represent an outcome of test.
Classification is done through a sequence of test until we reach a **leaf node**.

![[Pasted image 20250306101502.png]]

Easiest method to build a decision tree is **Top Down Induction of Decision Tree (TDIDT)**.
There are many variants of this method: ID3, CART, C4.5...

In the example we focus only on **ID3** (Iterated Dicotomized Tree):

1. If All samples in $S$ belong to the same class $C$ then:
	1. Make a leaf node labeled $C$
2. Otherwise:
	1. Select the "most informative" attribute $A$
	2. Partition $S$ according to attribute $A$
	3. Recursively construtct subtrees $T_{1},T_{2}\dots$, for the subsets of $S$

![[Pasted image 20250306104432.png]]

How do I select the "most informative attribute"?
I need to select attribute which partitions the learning set into subsets as “**pure**” as possible (i.e. homogeneous in terms of class label).
There are various measure of "purity":
 - **Information Gain** (Entropy-based)
 - **Gini Index** (Impurity-based)
 - **Gain Ratio** (Normalized Information Gain)
**Entropy** measures uncertainty in a dataset.
$$
  Entropy(S) = - \sum p(c) \log_2(p(c))
$$**High entropy** → More uncertainty (mixed classes).  **Low entropy** → Less uncertainty (mostly one class).

**Information Gain**: Reduction in entropy after a split.  
$$
  Gain(A) = Entropy(S) - \sum \left( \frac{|S_v|}{|S|} \right) Entropy(S_v)
$$

**Higher gain** → Better attribute for splitting.

**Gini Index** measures impurity of a dataset.
$$
  Gini(S) = 1 - \sum p(c)^2
$$**Low Gini** → More homogeneous groups. **Gini Gain** is used to choose the best attribute.
Gini Ratio Normalizes **Information Gain** to prevent bias towards attributes with many values.$$
  Gain Ratio(A) = \frac{Gain(A)}{SplitInfo(A)}
$$**SplitInfo(A)**: Measures the number of partitions created by an attribute.
  - **Used in C4.5 algorithm** to improve decision tree splits.

Summary:
- **Entropy & Gini Index** help measure how "pure" splits are.
- **Information Gain** prefers attributes that separate data well.
- **Gain Ratio** prevents bias toward attributes with many values.
- Choosing the right measure improves **tree accuracy** and **reduces overfitting**.

## 4.6 Ensemble Methods

The idea is to have a group of **base learners**, which are combined to achieve an higher accuracy. If we want to merge different algorithms we need to remember that each algorithm has its own assumptions, representation, training set, parameters...
So we need to combine the outcome of each model by **majority voting** or **weighed sum**.

![[Pasted image 20250306110420.png]]


The main challenge is not to obtain highly accurate base models, but rather to **obtain base models which make different kinds of errors**. For example, if ensembles are used for classification, high accuracies can be accomplished if different base models misclassify different training examples, even if the base classifier accuracy is low.

Independence between two base classifiers can be assessed by measuring the degree of **overlap in training examples they misclassify**: 
$$
|A \cap B| / |A \cup B|
$$
More overlap means less independence between two models.


### Bagging (Bootstrap Aggregating)  

Bagging creates different training sets by randomly resampling the original dataset. Each base learner is trained on a bootstrapped training set, and the final classifier is obtained by majority voting. This method reduces variance and stabilizes models like decision trees, preventing overfitting.  

Given a standard training set $D$ of size $n$:  
1. For $i=1$ to $M$:  
	1. Draw a sample of size $n* < n$ from $D$ (with replacement).  
	2. Train classifier $C_i$ on this sample.  
2. The final classifier is a majority vote of $C_1 ... C_m$.  

### Boosting  

Boosting aims to build a strong learner by combining several weak learners. A strong learner can be arbitrarily accurate but is difficult to construct. A weak learner is only slightly better than random guessing and is easy to construct.  

### AdaBoost (Adaptive Boosting)  

Unlike bagging, AdaBoost assigns weights to training samples instead of resampling. The algorithm starts by assigning equal weights to all samples. A weak classifier $C_i$ is trained, and misclassified samples receive increased weights while correctly classified ones receive decreased weights. This process is repeated for $k$ iterations, and the final classifier is a weighted vote of all classifiers. Final classifier formula:  $$
C_{final}(x) = \sum_{i=1}^{k} \alpha_i C_i(x)
$$where $\alpha_i$ represents the importance of classifier $C_i$, and $C_i(x)$ is the weak classifier prediction. Error rate:  
$$e_i = \sum_{j=1}^{N} w_j \delta(C_i(x_j) \neq y_j)$$
Classifier importance:  
$$\alpha_i = \frac{1}{2} \ln \left( \frac{1 - e_i}{e_i} \right)$$
AdaBoost reduces both bias and variance, making it more effective than bagging. However, it is sensitive to noisy data and can overfit on outliers.  

### Random Forest  

Random Forest is an ensemble method specifically designed for decision trees. It builds multiple unpruned decision trees, and the final classification is determined by majority vote over all trees.  

![[Pasted image 20250306111821.png]]

How does it work?
Bagging is used to train each tree on a bootstrapped dataset. At each node, a random subset of attributes is selected for splitting.  

1. Grow $M$ decision trees.  
2. For each tree:  
	1. Select a random subset of the training data.  
	2. At each split, choose the best split from $m$ randomly selected features.  
3. Final classification is determined by majority voting over all trees.  

Methods for Growing the Trees:
1. Randomly select $m$ attributes and choose the best attribute based on information gain.  
2. Compute information gain for all $M$ attributes, select the top $m$ attributes, and randomly pick one as the splitting node.  
