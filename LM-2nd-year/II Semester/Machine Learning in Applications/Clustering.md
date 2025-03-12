# 3. Clustering

**Cluster analysis** or **clustering** is the task of grouping a set of objects in such a way that objects in the same group (called a cluster) are more similar (in some specific sense defined by the analyst) to each other than to those in other groups (clusters).

In clustering, we often deal with **Proximity** of data, that is a term to refer to **similarity or dissimilarity**. Both of these measures how alike or not alike are two datapoints. Often similarity has a value range of $[0,1]$, while the upper level o dissimilarity varies. **Distance** is sometime used as synonym of dissimilarity.

## 3.1 Partitional Clustering 

The main idea of partitional (flat) clustering is iteratively **re-assign points** to a finite set of **disjoint** clusters.

### K-Means

For a given cluster assignment $C$ of data points, we compute the cluster's **centroid** $m_{k}$ as:
$$

$$
For the current set of cluster means, we assign each observation to the closest centroid, then we re-iterate the process until **convergence**.
This approach is called **SSE** (sum of the squared distance).

![[Pasted image 20250305131846.png]]

The most naive form of K-means demonstrates some disadvantages: 
- K-means is strongly dependent on **initialization**, so we may need to run the algorithm multiple times
- The algorithm is **sensitive to outliers**: to mitigate this we can use k-medians variant 
- How can we decide $k$? Either we have a prior knowledge on the problem or we could simply try different value of $k$ and plot the results. ![[Pasted image 20250305132410.png]] For example in the plot above $k=2$ is one the best value, corresponding to the "elbow finding".

## 3.2 Hierarchical Clustering

The main idea of hierarchical clustering is iteratively **merge or split a set of nested clusters**, organized into a hierarchical tree. Can be visualized as a **dendrogram**, which is a tree like diagram that records the sequence of merges or splits.

The main advantages of hierachical clustering are:
- We do not have to decide any particular number of clusters: we can simply **cut the dendrogram** at a certain level (going from hierarchical to partitional).
- The dendrogram **may correspond** to meaningful taxonomy (e.g. animal kingdoms or in biological science) 

We have two main approaches to perform hierarchical clustering:
- **Agglomerative approach**: starting with each point as an individual cluster, at each step we merge two cluster that are "closer" to each other until a stopping criteria
- **Divisive approach**: the opposite, starting from a single cluster, at each step we divide into two sub-cluster

Considering a naive agglomerative cluster algorithm we have the following steps:
1. Compute the proximity matrix
2. Let each data point as a cluster
3. Repeat
	1. Merge the two closest clusters
	2. Update the proximity matrix
4. Until a single cluster is reached

In both agglomerative and divisive approach we need to define the a criteria to define which are the **best pair of cluster**. We have different options: MAX, MIN, Group Average, Distance of centroids, Others...

| Strategy      | Definition                                                                                                                                                                  | Pros                                    | Cons                                                            |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------------------------------------------------------- |
| MIN           | Similarity of two clusters is based on the two most similar (closest) points in the different clusters                                                                      | Can handle non-elliptical shapes        | Sensitive to noise and outliers                                 |
| MAX           | Similarity of two clusters is based on the two least similar (most distant) points in the different clusters                                                                | Less susceptible to noise and outliers  | Tends to break large clusters. Biased towards globular clusters |
| Group Average | Proximity of two clusters is the average of pairwise proximity<br>between points in the two clusters. It is a compromise between single-link (MIN) and complete-link (MAX). | Less susceptible to noise and outliers. | Biased towards globular clusters                                |
Overall problems of hierarchical clusteing:
- Once a decision is made to combine two clusters, it cannot be undone.
- No objective function is directly minimized.
- Different schemes have problems with one or more of the following:
	1. Sensitivity to noise and outliers
	2. Difficulty handling different sized clusters
	3. Breaking large clusters
- Inherently unstable towards addition or deletion of samples.

## 3.3 Density-based Clustering

The idea is to partitions data based on **density**.

### Mean Shift Clustering

In this algorithm, data points are interpreted as they were sampled from a probability distribution. Mean Shift (MS) is built upon the concept of **kernel density estimation** (KDE).
KDE is useful to estimate the underlying probability distribution of some data.

How does KDE works?
It works by placing a **kernel** on each point of the dataset in the feature space. Adding up all the individual kernels we obtain a **density function**. Most popular kernel is the Gaussina kernel:
$$
K\left( \frac{x-x_{i}}{h} \right) = \frac{1}{\sqrt{ 2\pi }} e^{- (x-x_{i})^2/2h^2}
$$
Let's imagine a 2D surface, I can apply small Gaussian to each point, then sum it up and gain a density function:

![[Pasted image 20250305135834.png]]

Having then 3 peaks, it means that we obtained 3 clusters.
Mean shift exploits this KDE idea by imagining what the points would do if they all climbed up hill to the nearest peak on the KDE surface. It does so by iteratively shifting each point uphill until it reaches a peak. Hence, all the points are clustered depending on the peak they are shifted to.

- Compute mean shift vector $m(x)$. The mean shift vector always points toward the direction of the maximum increase of density
- Translate the Kernel window by $m(x)$. ![[Pasted image 20250305140410.png]]

Pros of Mean Shift Clustering:
- There is no need to apriori decide the number of clusters
- Robust to outliers
- Quite flexible, no assumptions on the data distribution

Cons of Mean Shift Clustering:
- Computationally intensive
- Not feasible for high-dimensional dataset
- Results depend on kernel/bandwidth

### DBSCAN

DBSCAN is a **density-based cluster** algorithm. In this context the density refers to the number of points within a specified radius **Eps**.
We can distinguish 3 categories of points:
- **Core point**: A point is a core point if it has more than a specified number of points (MinPts) within Eps.
- **Border Point**: A border point has fewer than MinPts within Eps, but is in the neighborhood of a core point.
- **Noise Point**: neither a core or border point

![[Pasted image 20250305141145.png]]

The algorithm works as follow:
1. Label all points as core, border or noise points.
2. Eliminate all noise points
3. Put an edge between all core points that are within Eps of each other
4. Make each group of connected core points into a separate cluster
5. Assign each border point to one of the clusters of its associated core points.

![[Pasted image 20250305141431.png]]

| Pros                                                                    | When DBSCAN does not work well             |
| ----------------------------------------------------------------------- | ------------------------------------------ |
| resistant to noise<br>can handle clusters of different shapes and sizes | varying densities<br>high-dimensional data |

Usually to determine Eps and MinPts we optimize Eps by fixing MinPts:
- for points in a cluster, their k-th nearest neighbors are at roughly the same distance
- noise points have the k-th nearest neighbor at farther distance
- plot sorted distance of every point to its k-th nearest neighbor
Choose Eps based on the "elbow finding".

## 3.4 Cluster Validity

For clustering, the analogous question is how to **evaluate the “goodness”** of the resulting clusters?

1. Determining the **clustering tendency** of a set of data, i.e., distinguishing whether non-random structure actually exists in the data.
2. Comparing the results of a cluster analysis to **externally known results**, e.g., to externally given class labels.
3. Evaluating how well the results of a **cluster analysis fit** the data without reference to external information. (Use only the data)
4. Comparing the **results of two different sets of cluster** analyses to determine which is better.
5. Determining the **‘correct’ number** of clusters.

Numerical measures used to judge various aspects of cluster validity are classified into the following three types:
- **External** index: Measures extent to which cluster labels match externally supplied class labels.
- **Relative** index: Compares two different clustering or clusters. Often an external or internal index is used for this purpose.
- **Internal** index: Measures the “goodness” of a clustering structure without respect to external information.

Some internal indexes are **cohesion and separation**:

![[Pasted image 20250305143005.png]]

**Silhouette** Coefficient combine ideas of both cohesion and separation, but for individual points, as well as clusters and clustering techniques.

![[Pasted image 20250305143130.png]]
