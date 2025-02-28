# Dimensionality reduction

The goal of dimensionality reduction is to perform a mapping from the $n$-dimensional feature space to a $m$-dimensional space, with $m \ll n$.

Different goal may require different approaches:
- Compress information: we want to retain the maximum amount of information for a given output size
- Improve classification (our goal): we want to retain discriminant information

We'll focus only on two methods: **PCA** (unsupervised) and **LDA** (supervised).

# Notes on Linear Algebra

Consider a n x n matrix $A \in \mathcal{R}^{n \times n}$:
A **admits a eigen-decomposition**:
$$
A = V\Sigma V^{-1} = V\Sigma V^T
$$
Where $V$ is n x n matrix whose columns are the **eigenvectors** of $A$.
$\Sigma$ is diagonal n x n whose element are the eigenvalues of $A$

A generic rectangular matrix $A \in \mathcal{R}^{n \times m}$ always admits a **SVD decomposition**:
$$
A = U\Sigma V^T
$$
Where $U$ is an orthogonal n × n matrix of eigenvectors of $A A^T$
$V$ is an orthogonal m × m matrix of eigenvectors of $A^TA$
$\Sigma$ is a diagonal rectangular matrix containing the singular values of $S$ (in decreasing order).

Projecting a point over a direction $u$:

![[Pasted image 20240321163733.png]]

Projecting a point $x$ in an $m$-dimensional subspace (defined by multiple direction $U=[u_{1},\dots u_{m}]$):

![[Pasted image 20240321164130.png]]

# Principal Components Analysis (PCA)

The goal is to find a linear mapping that preserve information in our data, reducing dimensionality.
We need to find a subspace in which we project our samples (we need to find a direction $u$ in which project our points $x_{i}$).
We can **represent a subspace** as a matrix $P \in \mathcal{R^{n \times m}}$ where columns are orthonormal.
The projection of samples of that subspace is:
$$
y = P^Tx
$$
Then we can reconstruct samples (in subspace coordinates) by computing $\hat{x} =  Py$.

Now the question is: how can we choose $P$?
One way to perform this operation is the **minimization of reconstruction operation** (minimize $\hat{x}- x$).
If $K$ is the number of samples we need to compute:
$$
P^* = \arg \min_{P} \frac{1}{K} \sum_{1}^K \lvert x_{i}- PP^Tx_{i} \rvert^2 
$$
Note that the term $x_{i}- PP^Tx_{i}$ is obtained from:
$$
x_{i} -\hat{x} = x_{i}- Py_{i} = x_{i}- PP^Tx_{i}
$$
Constraint: $P^TP = I$.

Now we want to compute this $P^*$: we can rewrite the objective function $\mathcal{L}(P)$

![[Pasted image 20240321165818.png]]

The term $Tr(P^Tx_{i}x_{i}^TP)$ represents the **trace** of a matrix.
The sum function is composed by two terms, the first one does not influence P, so we need to minimize $\frac{1}{K} \sum -Tr(P^Tx_{i}x_{i}^TP)$ : due to the presence of the minus sign we can compute the minimum by **maximize** $\mathcal{\hat{L}}(P)$:
$$
\mathcal{\hat{L}}(P) = \text{Tr} ( P^T\left[ \frac{1}{K} \sum_{i=1}^K x_{i}x_{i}^T \right]P)
$$
We require that $P$ is an **orthogonal** matrix.

It can be shown that the optimal solution is then given by the matrix $P$ whose columns are the $m$ **eigenvectors** of $\frac{1}{K}\sum x_{i}x_{i}^T$ corresponding to the $m$ largest eigenvalues.
This term is related to the **covariance matrix**, which will be our solution.

If data is not zero-mean (not normalized) the first PCA direction will connect the sample cluster with the origin of the subspace (which is not very interesting):

![[Pasted image 20240321171015.png]]

We can recast the original problem: now we want to look for a shift $m$ and a subspace $U$ where to project the data.
An optimal solution for the shift is **the dataset mean**.
In practice **we remove the dataset mean before applying PCA**.

The PCA subspace is computed from the eigenvalues of the empirical covariance matrix:
since we are computing projection on $U$ subspace, starting from covariance matrix $C$ and considering only the first $m$ directions, we obtain that the first $m$ directions have the highest variance.

Practical recap:
1. Compute sample mean $\bar{x}$
2. Center data $z_{i} = x_{i} - \bar{x}$
3. Compute the covariance matrix $$
C= \frac{1}{K} \sum_{i} (x_{i}- \bar{u})(x_{i} - \bar{u})^T = \frac{1}{K \sum_{i}}z_{i}z_{i}^T
$$
4. Compute eigen decomposition $C = U \Sigma U^T$
5. Project the data in the subspace spanned by the $m$ columns of $U$ corresponding the $m$ highest eigenvalues (matrix $P$): $y_{i} = P^T(x_{i}- \bar{x}) = P^Tz_{i}$
6. Reconstruct alla values: $\hat{x_{i}} = \bar{P}y_{i} = \bar{x}$

**Selection of optimal $m$** can be done by cross–validation using a validation set.
We can also select $m$ as to retain a given percentage t (e.g. 95%) of the variance of the data.
Remember that each eigenvalue corresponds to the variance along the corresponding axis.

To analyze the results of PCA as **pre-processing** for classification, we can use Euclidian distance:
$$
\mu_{c} = \frac{1}{n_{c}} \sum_{i}x_{c,i} \quad \quad c_{t} = \arg \min_{c} \mid x_{t}- \mu_{c}\mid^1
$$

# Linear Discriminant Analysis (LDA)

PCA does not guarantee **discriminant directions** (because it's an unsupervised technique).
**Linear Discriminant Analysis** (LDA) is a supervised technique that allows us to better separate classes: in order to do this we need to find a new direction with **large separation** of classes and **small spread** inside each class. 

In the following figure: RED line is PCA, while GREEN line is LDA.

![[Pasted image 20240404150435.png]]

We represent a direction with a unit vector $w$: we also need to define **Between and Within** class variability. We need $S_{W}$ to be small, so we have small spread within classes, while $S_{B}$ needs to be big in order to have separated classes.
$$
S_{B} = \frac{1}{N} \sum_{c=1}^K n_{c}(\mu_{c}-\mu)(\mu_{c}-\mu)^T
$$
$$
S_{W} = \frac{1}{N} \sum_{c=1}^K \sum_{i=1}^{n_{c}} (x_{c,i} - \mu_{c})(x_{c,i}- \mu_{c})^T
$$
where $x_{c,i}$ is the $i$-th sample of class $c$, $n_{c}$ is the number of samples of class $c$, $K$ is the total number of classes and $N$ is the total number of samples.
Note that the definition above are exactly the definition of covariance matrixes: $S_{B}$ is the covariance matrix of each class mean, weighted by the number of samples $n_{c}$. $S_{W}$ is the (weighted) average of covariance matrix of each class.

Since we are looking for a direction $w$ we can define the **global mean** as $m=w^T\mu$, while the mean for each class is $m_{c} = w^T\mu_{c}$
We can compute the between and within class variance over direction $w$:

![[Pasted image 20240404151856.png]]

We consider the ratio and we want to **maximize** it:
$$
\mathcal{L}(w) = \frac{s_{B}}{s_{W}} = \frac{w^TS_{B}w}{w^TS_{W}w}
$$
We can find an optimum by computing $\nabla_{w} \mathcal{L}_{w} = 0$, so we need to compute the **gradient**.
The optimal solution is the **eigenvector** of $S_{W}^{-1}S_{B}$ corresponding to the largest eigenvalue.

### LDA for binary classification

For classification of a binary problem we can express the **between class** covariance matrix as:
$$
S_{B} = k(\mu_{2}-\mu_{1})(\mu_{2}-\mu_{1})^T
$$ Where $k$ is a constant but it's irrelevant to find the optimal direction. In this particular case we can observe that the rank of $S_{B}$ is equal to 1, so $S_{W}^{-1}S_{B}$ has a single, non-zero, eigenvalue, which is:
$$
w \propto S_{W}^{-1}(\mu_{2}-\mu_{1}) 
$$
A simple classification rule is based on computed the distance between the sample and the mean of a class: the threshold value is just the centre of the two mean:
$$
C(x_{t}) = \begin{cases}
C_{1} \, \, \text{if} \, \, w^Tx_{t} < t && \\
C_{2} \, \, \text{if} \, \, w^Tx_{t} \ge t
\end{cases}
$$
Where $t =\frac{\mu_{1}+\mu_{2}}{2}$

### LDA for dimensionality reduction

In this case we need for $m$ discriminant dimension: these directions are represented as a matrix $W$, whose columns contain the direction we want to find.
We can recompute $\hat{S_{W}}$ and $\hat{S_{B}}$, then the solution is given by the $m$ right eigenvectors corresponding to $m$ largest eigenvalues.
In general, **LDA allows estimating at most $C-1$ directions** (due to rank of $S_{B}$).

A generalized way to solve $W$ matrix is described by the following steps:
1. Compute eigen value decomposition $S_{W} = U_{W}\Sigma_{W}U^T_{{W}}$
2. Apply **whitening** transformation $P_{W} = U_{W}\Sigma_{W}^{-1/2}U^T_{W}$
3. Compute eigen value decomposition $PS_{B}P^T = U_{B}\Sigma_{B}U_{B}^T$
4. Diagonalize by projection over $U_{B}^T$

The first $m$ direction of the transformed samples correspond to the **LDA subspace**.
# Non-linear dimensionality reduction

Linear transformation are not always suited for data: in that case we need to perform a transformation on data, in order to make linear method suited for the transformed data.
One example is using polar coordinates:
$$
x = \begin{bmatrix}
x_{1} \\
x_{2}
\end{bmatrix} \to y = f(x) \begin{bmatrix}
\rho(x) \\ 
\theta(x)
\end{bmatrix}
$$
![[Pasted image 20240404163321.png]]

In the figure above, PCA and LDA are useless in first coordinates system: we need polar coordinates to make them meaningful.

Next: [[Probability and density estimation]].