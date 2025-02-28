# 2. Feature Reduction

In this first section we will focus on data preprocessing: the goal of this phase is the creation of a training and testing datasets. To do so, we need to collect raw data and apply transformation, normalization and other techniques to **make data more tractable**.

An intuition of **feature reduction** is: summarize data with many ($n$) variables by a smaller set of ($m$) derived variables.
Why is this useful?
- Address the **curse of dimensionality** 
- The **intrinsic** dimension of the problem is small (for example, only few genes are responsible for some disease)
- **Visualization** is tractable in 2D or 3D
- **Data compression** provides efficient storage and retrieval
- **Noise removal**: positive effect on query accuracy
We need to find a trade-off between ease-of-representation and losing relevant information.

To perform feature reduction we have two main approaches:
- **Feature Selection (FS)**: based on choosing a subset of original feature
- **Dimensionality Reduction (DS)**: based on mathematical recombination of original features

| Feature Selection                                                                                                                                                                 | Dimensionality Reduction                                                                                                    |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| equivalent to projecting feature space to a<br>lower dimensional subspace perpendicular to removed<br>feature, which is a subset of the original one                              | allows other kinds of projection.The new space is not a subset, but a recombination of the<br>original feature space.       |
| ![[Pasted image 20250226144624.png]]                                                                                                                                              | ![[Pasted image 20250226144649.png]]                                                                                        |
| When classifying novel patterns, only a small number of features need to be computed (i.e., faster classification). The measurement units of the original features are preserved. | When classifying novel patterns, all features need to be computed. The measurement units of the original features are lost. |
## 2.1 Dimensionality Reduction Methods

### Principal Component Analysis (PCA)

**PCA** is a dimensionality reduction technique that aims to reduce the dimensionality of data. It is an exploratory and **unsupervised** technique that:
- From a matrix of $m$ samples x $n$ features, create a new covariance matrix of size $n$ x $n$.
 - Transform some large number of variables into a smaller number of **uncorrelated variables** called **principal components** (PCs).
- Developed to **capture as much of the variation** in data as possible

The new variables in the feature space are linear combination of the original axes, in order to maximize the variance of data. This approach is based on the **eigenvalues decomposition** of the data covariance matrix.
The first $m<n$ dimensions are considered to be the most representative, so they allow to preserve information.

![[Pasted image 20250226150137.png]]

How can we measure the spread of data along dimensions? The variance of data can be computed as:
$$
\text{cov}(X,Y) = \frac{\sum_{i}^n(X_{i}-\bar{X})(Y_{i}-\bar{Y})}{n-1}
$$
When dealing with high dimensional data, i can compute the covariance matrix between each pair of features:
$$
C^{n \times n} = (c_{ij}|c_{ij} = \text{cov}(Dim_{i}, Dim_{j}))
$$
The eigenvalue with the largest absolute value will indicate that the data have the largest variance along its eigenvector, the direction along which there is greatest variation.
The ranking of directions can be computed by looking at the magnitude of eigenvalues.
Note: in general, only few directions manage to capture most of the variability of data.

To compute PCA in practice:
1. Mean-center the data
2. Compute the covariance matrix
3. Compute eigenvalues and eigenvectors
4. Compute the variance of each PCs: $V_{j}=100 \cdot \frac{\lambda_{j}}{\sum_{i}\lambda_{i}}$
5. Filter the irrelevant dimensions with lower covariance

### Linear Discriminant Analysis

PCA is unsupervised (does not take into account class information)
**LDA** instead, is a **supervised** method that find a low-dimensional space
such that when $x$ is projected, **classes are well-separated**. In other words LDA maximize inter-class distance and also minimize intra-class variance.

![[Pasted image 20250226151841.png]]

### Kernel PCA (KPCA) and KDA

In many cases, data may not be best summarized by a linear combination of features (classical example: PCA cannot discover the 1-D structure of an helix). In general, linear projection such as PCA and LDA do not detect non-linear patterns.

A possible solution is to **apply a non-linear** transformation to potentially very high dimensional space, where it is possible to obtain preferential direction linearly.
$$
\Phi : x \to \Phi(x)
$$
This approach is known as the **kernel trick**. Instead of explicitly computing the transformation, which would be computationally expensive (or even infeasible) in high-dimensional spaces, we rewrite algorithms in terms of **dot products**. This allows us to work indirectly in the transformed space without explicitly performing the transformation, thanks to specially designed **kernel functions** that compute these dot products efficiently.

![[Pasted image 20250226153018.png]]

**Kernel PCA (KPCA)** applies this concept by first transforming the data using a kernel function and then performing PCA in the transformed space. This allows it to capture directions of maximum variance even in cases where traditional PCA fails. Once a solution is found in the high-dimensional kernel space, the results can be mapped back to the original space if needed.

A similar approach can be applied to **Linear Discriminant Analysis (LDA)**, resulting in **Kernel Discriminant Analysis (KDA)**.

![[Pasted image 20250226153303.png]]

However, a key challenge with kernel-based methods is the choice of the kernel function itself. Since there is no universal best choice, selecting an appropriate kernel often requires empirical experimentation and domain knowledge.

### Isomap

A **manifold** is a topological space that is **locally Euclidean**, meaning that in small neighborhoods, it behaves like standard Euclidean space. While raw data is often represented in a high-dimensional space, it may actually originate from an underlying process that depends on only a few degrees of freedom. In such cases, the data lies on a lower-dimensional **manifold** within the high-dimensional space, and appropriate transformations can help us recover this intrinsic structure.

![[Pasted image 20250226153826.png]]

A key approach in **manifold learning** is **Isomap**, a nonlinear dimensionality reduction technique that estimates the underlying geometric structure of the data by considering the relationships between nearby points. Unlike traditional Euclidean distance, which may not accurately reflect the true similarities between points in a curved space (e.g., a helical structure), Isomap instead relies on **geodesic distances**, which measure the shortest path along the manifold.

![[Pasted image 20250226154511.png]]

How to compute geodesic distances? First, we need to compute a neighborhood graph by applying KNN to data, discovering close nodes, that the edges can be computed by euclidean distance (manifold suppose local euclidean distance). Then once we have the graph we can calculate shortest path between nodes to get the geodesic distance. We can build a geodesic matrix and applying eigenvalues decomposition on it.
Disadvantages: if $N$ is small, geodesic distances will be inaccurate, while if $N$ is large then the computational cost is higher

### Locally Linear Embedding (LLE)

Similar to **Isomap**, this approach first constructs a **neighborhood graph** using **k-Nearest Neighbors (KNN)** to identify local relationships between points. Once the neighborhood structure is established, each point is **reconstructed** as a weighted linear combination of its neighbors. Finally, the data is mapped into a lower-dimensional space while preserving these local relationships.

The key idea behind this method is to approximate a **nonlinear manifold** by treating it as a collection of small, locally **linear** regions. This means that, within each neighborhood, the data structure is assumed to be approximately linear, allowing for simpler transformations.

![[Pasted image 20250226155519.png]]

Visually, this process is similar to applying **PCA** within each local region, identifying a linear subspace (a hyperplane) that best represents the data in that small neighborhood. These local representations are then **stitched together globally** to find an optimal nonlinear embedding for the entire dataset.

However, this method has limitations: it tends to perform poorly when dealing with **datasets that contain multiple disconnected manifolds** or highly complex structures—such as **MNIST**, where handwritten digits exhibit substantial variability and overlapping patterns.
### t-distributed Stochastic Neighbor Embedding (t-SNE)

Instead of describing the relationship between points with distances, we use **probabilistic similarity**. Given n datapoints, the similarity of datapoint $x_{j}$ to datapoint $x_{i}$ depends
the conditional probability $P(j|i)$ that $x_{i}$ would pick $x_{j}$ as its neighbor, if neighbors
were picked in to their density under a SNE converts euclidean distances to proportion
similarities, that can probability be Gaussian centered at $x_{i}$.

![[Pasted image 20250227102723.png]]

It’s important to note that **t-SNE** preserves **pairwise similarities** between datapoints. This makes it particularly effective for **visualization** and for uncovering complex structures in data, especially when dealing with non-linear manifolds. However, the main drawbacks are its **high computational cost**, especially with large datasets, and the potential to learn patterns that may not actually exist in the data. Additionally, **global structure** is not preserved, meaning t-SNE is not suitable for tasks that require maintaining overall distances or relationships between points in high-dimensional space.

## 2.2 Feature Selection 

Feature selection is an **optimization process** where we search the space of possible feature subsets, and then we pick the optimal or near-optimal subset based on some **objective function**. 

![[Pasted image 20250227104918.png]]

To actually evaluate the quality of a subset we have two strategies: filter method or wrapper method.

The **wrapper method** relies on criteria specific to the classification algorithm. The objective function is a pattern classifier, which evaluates feature subsets based on their predictive accuracy. While this method generally achieves better recognition rates, its main drawback is the **long execution time** and a **lack of generality** due to its reliance on a specific classifier.

On the other hand, the **filter method** is independent of the classification algorithm. Instead of focusing on classification performance, it evaluates feature subsets by measuring their **information content**, such as **inter-class distance**, **statistical dependence**, or other **information-theoretic measures**. The main advantage of this method is its **faster execution time** and **better generality** across different tasks. However, it tends to select **larger subsets of features**, which may not always be ideal for model efficiency.
### Search Strategies

First, we need to search the space of feature subsets. Since an exhaustive search of all possible subsets is computationally expensive, we use different search strategies, which can be based on either **heuristic search** or **non-deterministic search**.

A **Naive search** simply trains a ML model with one single individual feature and measure the objective function. Then we sort the $n$ features in order of the classification accuracy obtained with each of them, and choice the best $d$ features. This approach **does not take in account the relationship** within features.

**Sequential Forward Search (SFS)** iteratively construct a subset: First, the best single feature is selected (i.e. using classification accuracy as criterion function).
Then, pairs of features are formed using one of the remaining features and this best feature, and the best pair is selected.
Next, triplets of features are formed using one of the remaining features and these two best
features, and the best triplet is selected.
This procedure continues until a predefined number of features are selected, or even after
ALL of them have been selected.
This procedure performs best  when the optimal subset is small.

![[Pasted image 20250227105836.png]]

**Sequential Backward Search (SBS)** works by considering the full set and removing one at a time the worst feature. SBS performs well when the optimal subset is large. No guarantee of finding same solution of SFS. Also if we have no clue on the optimal size of the subset SFS and SBS are equivalent.

![[Pasted image 20250227110020.png]]

- The main limitation of SFS is that it is unable to remove features that become non useful after the addition of other features.
- The main limitation of SBS is its inability to re-evaluate the usefulness of a feature after it has been discarded.

To mitigate this problems we have combined approaches:

**Bidirectional Search (BDS)** performs at the same time SFS and SBS: to guarantee convergence to the same solution we need to ensure that feature selected by SFS are not removed by SBS and the opposite.

**Plus-L Minus-R Selection (LRS)** works with 2 hyperparameters $L,R$ that regulate the search and it proceeds as follow.
If $L > R$ we start from an empty set and:
- Repeatedly add $L$ features
- Repeatedly remove $R$ features
If $L < R$ we start from a full set and:
- Repeatedly remove $R$ features
- Repeatedly add $L$ features

An extension of LRS is called **Sequential floating Backward/forward Selection (SFFB or SFBS)**: Rather than fixing the values of L and R, floating methods determine these values from the data.

### Genetic Algorithms

To effectively explore the space of feature subsets, we can introduce randomness in the selection process using **Genetic Algorithms (GAs)**. Genetic Algorithms are a class of optimization techniques inspired by the process of natural selection, where solutions evolve over time through operations such as selection, crossover (recombination), and mutation.

![[Pasted image 20250227111307.png]]

In the context of feature selection, we can represent each possible feature subset as a **chromosome** using a binary encoding. In this encoding, each feature is represented by a bit: 0 indicates that the feature is not selected, and 1 indicates that the feature is selected. The algorithm then iterates through several generations of solutions, evolving the population of feature subsets towards an optimal or near-optimal solution.

Key components of Genetic Algorithms:
1. **Initial Population**: A set of randomly generated feature subsets (chromosomes), which represent potential solutions to the problem.
2. **Fitness Function**: Each chromosome is evaluated using a fitness function, which determines how good the solution is. For feature selection, the fitness function can be a combination of classification accuracy and the number of selected features, to balance both model performance and feature efficiency:$$
    \text{fitness} = w_{1} \cdot \text{accuracy} + w_{2} \cdot \text{number of discarded features}$$Here, $w_{1}$ and $w_{2}$ are weights that control the importance of classification accuracy and feature efficiency.
3. **Selection**: The best-performing individuals (feature subsets) are selected for reproduction. Selection can be based on their fitness, with higher fitness individuals having a higher chance of being selected. This mimics the survival of the fittest in natural selection.
4. **Crossover (Recombination)**: Selected individuals are paired to exchange genetic material (feature subsets). This creates new offspring by combining the features of the parent subsets. The idea is to combine beneficial features from different subsets, which might lead to better solutions.
5. **Mutation**: Occasionally, random changes are introduced into an offspring's chromosome by flipping bits (i.e., changing selected features from 1 to 0 or vice versa). Mutation ensures diversity in the population and helps the algorithm escape local optima.
6. **Survival**: After generating offspring through crossover and mutation, the population is updated by selecting individuals for survival. This can involve keeping the best individuals from the current population and the new offspring, ensuring the population improves over generations.

The Genetic Algorithm continues iterating through generations, with each generation producing better feature subsets until a stopping condition is met (e.g., reaching a maximum number of generations or achieving a satisfactory fitness level).

This process introduces randomness into the feature selection, allowing the algorithm to explore the solution space more effectively and find feature subsets that might not be discoverable through deterministic approaches.