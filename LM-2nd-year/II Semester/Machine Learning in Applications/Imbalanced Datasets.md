# 5. Imbalanced Datasets

## 5.1 The problem of imbalanced datasets

Nowadays, we have an explosive increment in the amount of data, along with a bunch of well-developed algorithms for data analysis. We might want to choose a supervised approach. However there are some constraints that we need to consider. In order to learn a good classifier we need a **balanced distribution** of data. Indirectly we are also assuming an **equal cost of misclassification**.

In real applications, it's very hard to have a balanced distribution of data. Imbalance affects several aspect of the learning process:
- Imbalance between classes
- Within classes
- Intrinsic and extrinsic
- Relativity and rarity
- Imbalance and small sample size

It is very common in real-world machine learning applications to have **between-class** imbalance, for example in medical field:

![[Pasted image 20250312132341.png]]

We can split the definition of imbalance into two main part:
- **intrinsic** imbalance, typically due to the nature of the dataspace (e.g. the example above)
- **extrinsic**: Imbalance due to time, storage, and other factors (e.g. transmission of data with some interruption)

If the minority class is outnumbered, but still have a good number of samples, then we talk about **relative imbalance**.
$$
Q:100000:1000 = 1000:1
$$
So in the first part of the equation we can accurately learn with a little disturbance. In the second case we talk about **absolute rarity**.

We also need to take in account the **data complexity**: for example both majority and minority classes are distributed in a complex way, and it is also possible to identify sub-classes. So a ML model needs to consider all these factors.

![[Pasted image 20250312133138.png]]

Lastly, in real-world situation we have **imbalanced data with small sample size**.Data with high dimensionality and small sample size (e.g. Bioinformatics: gene expression analysis, histopathology). The main challenges with small sample size are 
1. Embedded absolute rarity and within-class imbalances
2. Failure of generalizing inductive rules by learning algorithms

## 5.2 Solutions to imbalanced datasets

In literature, we can identify three main categories of methods to address the challenge of imbalance:
- **Sampling** methods: this is the simplest form of method: if the data is imbalanced, then it simply works by modifying the distribution of data to get a balanced datasets. In other words we artificially generate a balanced dataset.
- **Cost-effective** methods: instead of modifying the distribution of data, we try to assign a cost of misclassification, then utilize cost-sensitive learning framwork
- **Kernel-based** methods
### Random sampling

**Random sampling** works by expanding or shrinking the minority/majority of data.

![[Pasted image 20250312133914.png]]

### Informed Undersampling

#TODO

### Synthetic Sampling with Data Generation

The main idea is to generate new samples for the minority class. Also called **SMOTE** (Synthetic minority oversampling technique). Minority class samples are used as **seeds** to generate new ones. Similarity between real minority samples and generated ones is represented as euclidean distance.
For each sample belonging to the minority class:
1. Randomly choose one of the $k$-nearest neighbor
2. Create a new sample starting from this sample

![[Pasted image 20250312135251.png]]

The main limitation of SMOTE is **over-generalization**: it generates same number of artificial samples per each original minority datapoint, without taking into consideration their neighborhood.

### Adaptive Synthetic Sampling

To overcome the limitation of SMOTE we can a slightly modified version called **Border-line SMOTE**:
![[Pasted image 20250312135509.png]]
![[Pasted image 20250312135615.png]]

Another version of SMOTE is called **ADASYN**:
![[Pasted image 20250312135647.png]]

### Cost-sensitive Methods

First thing to do is to define a **misclassification cost** as:
$$
C(\text{predicted}, \text{actual})
$$
In the case of assigning a majority sample to a minority we have $C(min,maj)$. Typically we have a higher cost of assigning a minority to a majority (e.g. in the medical field) so:
$$
C(maj,min) > C(min,maj)
$$
The learning process is then optimized to **minimize the Bayes Conditional risk**:
$$
R(i|x) = \sum_{j}P(j|x)C(i,j)
$$
In **Cost-sensitive Adaboost** we insert the misclassification cost (from the cost matrix) into the weighting scheme of the boosting strategy. This increase the probability of sampling a datapoint in the next iteration proportionally to its cost of misclassification (different variants available). It also impose a bias towards the minority class, as well as to the most relevant data samples.

In the context of ANN we could:
- **Modify the learning rate** so that it is higher for costly examples and lower for low-cost examples
- Replace the **error-minimizing function**, using expected cost-minimization instead

However, i many situations, the **actual explicit misclassification costs are unknown**, there is just a general understanding that misclassifying a minority class sample is more costly than misclassifying a majority class one!

### Kernel-based methods

The main problems with Kernel-based support vector machines (SVMs) are:
1. Support vectors from the minority concept may contribute less to the final hypothesis
2. Optimal hyperplane is also biased toward the majority class

A possible approaches to solve the problem:
1. Combination with sampling techniques
2. Modification of the soft-margin maximization paradigm with extra components taking data distribution skew into account (kernel modification methods)

## 5.3 Performance Metrics recap

**Singular assessment** metrics can be applied to a calibrated model: in the case of binary classification we can produce a **confusion matrix**.
$$
\text{accuracy} = \frac{TP+TN}{TP+TN+FP+FN}
$$
$$
\text{error rate} = 1- \text{accuracy}
$$
$$
\text{precision} = \frac{TP}{TP+FP}
$$
$$
\text{recall} = \frac{TP}{TP+FN}
$$
$$
\text{F-}\beta = \frac{(1+\beta^2)\cdot \text{recall} \cdot \text{precision}}{\beta^2}\cdot \text{recall} \cdot \text{precision} 
$$
$$
\text{G-Mean} = \sqrt{ \text{recall} \cdot \frac{TN}{TN+FP}}
$$

Assuming that the positive class is the relevant one:
- **precision** (also called positive predictive value PPV) is the fraction of selected instances that are actually relevant
- **recall** (also known as **sensitivity**) is the fraction of relevant instances that are selected by the classifier

Precision is a measure of "how useful the results are", and recall is "how complete the results are” In a skewed distribution, the convention is that the minority class is also the relevant (positive) one.

If I have to calibrate a model, I can test it ad different experimental setup. It is useful to plot **Receiver Operating Characteristics (ROC) curves**.

![[Pasted image 20250312143756.png]]

ROC curve tends to provide an overly optimistic view of the classifier performance in the case of highly imbalanced datasets.

We can also plot a **recall vs precision curve**: A curve dominates in ROC space (resides in the upper-left hand) if and only if it dominates (resides in the upper-right hand) in PR space.

![[Pasted image 20250312144013.png]]

**Cost curves** plot the expected cost of misclassification E(C) at different sample distributions, expressed as the probability of an example of being from the positive
class: p(+)
In the setting where the misclassification costs of the two classes are the same, the y-axis is simply representing the error rate.
The concept can also be generalized to different misclassification costs.