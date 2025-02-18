# SVM

With Logistic regressione we have seen that is useful to add a **regularization term**: We now consider a different classifier that allows us to give a geometrical interpretation of the regularization term: the Support Vector Machine
We will show that SVM can be cast as a **generalized risk minimization problem**.

SVMs provide a natural way to achieve non–linear separation without the need for an explicit expansion of our features.

In contrast with LR, however, the output of SVMs **cannot be directly interpreted as class posteriors**.

### Introduction

Let's consider two classes that can be linearly separated: LR would have no solution (norm term will be infinite).
Intuitively we can select the **hyperplane** that has the largest **margin**:

![[Pasted image 20240530155837.png]]

Let $f(x) = w^Tx +b$ the function that represents the separation surface (the hyperplane).
The distance of samples from the hyperplane is:
$$
d(x_{i}) = \frac{|f(x_{i})|}{\lvert \lvert w \rvert  \rvert } = \frac{|z_{i}(w^Tx_{i} + b)|}{\lvert \lvert w \rvert  \rvert }
$$
The maximum margin hyperplane is the hyperplane which **maximizes the minimum distance of all points from the hyperplane**:
![[Pasted image 20240530160417.png]]
![[Pasted image 20240530160506.png]]
We obtain that for values $w^*, b^*$ which can correctly separate the classes we have:
$$
z_{i}(w^Tx_{i}+b) >0  \quad \quad \quad \min_{i} z_{i}(w^Tx_{i}+b) > 0
$$
### Optimization

Now the goal is the **optimization of (1)**, which is non-trivial: we note that this function is **invariant under rescaling of parameters**: (for each $\alpha > 0$):
![[Pasted image 20240530161100.png]]
If $(w^*,b^*)$ is an optimal solution, also $(\alpha w^*, \alpha b^*)$ is an optimal solution and viceversa.
A set of optimal solutions $\{(\alpha w,\alpha b)\}$ is called **equivalence class**. We restrict our problem to solutions for which:
$$
\min_{i} z_{i}(w^Tx_{i}+b) = 1 \quad \quad \quad z_{i}(w^Tx_{i}+b) \geq 1
$$
The problem in (1) now is the following:
![[Pasted image 20240530161920.png]]
Or equivalently, minimizing the squared norm.


The goal of SVM is finding the minimizer of:
$$
\arg \min_{w,b} \frac{1}{2} \lvert  \lvert w \rvert  \rvert^2 \quad 
\text{such that} \quad z_{i}(w^Tx_{i} + b) \geq 1
$$
This is unfortunately, a convex quadratic programming problem, thus we consider a **lagrangian formulation** of the problem:
![[Pasted image 20240530162635.png]]
The optimal solution consists of minimizing $L$ with respect to $w,b$ and require that the derivatives with respect to $\alpha_{i}$ vanish (remember that $\alpha_{i} > 0$). Since this is a convex problem we can **maximize** $L$ with respect to $\alpha_{i}$ and require that the derivatives (w.r.t $w,b$) vanish.
The constraint of the derivatives (with respect to $w,b$) gives:
$$
w = \sum_{i=1}^n \alpha_{i}z_{i}x_{i} \quad \quad \quad 0 = \sum_{i=1}^n \alpha_{i}z_{i}
$$
Replacing this is $L$ we obtain the dual SVM problem:
$$
\begin{cases}
\max_{\alpha} \sum_{i=1}^n \alpha_{i} -\frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n \alpha_{i} \alpha_{j} z_{i} z_{j} x_{i}^T x_{j} \\
\sum_{i=1}^n \alpha_{i} z_{i} = 0
\end{cases}
$$
The dual objective function can be expressed with matrix $H = z_{i}z_{j}x_{i}^Tx_{i}$:
![[Pasted image 20240530163419.png]]
Let's call **primal objective** the function $L_{P}(w,b) = \frac{1}{2} \lvert  \lvert  w \rvert \rvert^2$.
We define the difference as **duality gap**:
$$
L_{P}(w.b)-L_{D} ( \alpha ) \geq 0 
$$
An optimal solution requires the duality gap to be zero.
A solution $(w,b, \alpha)$ will be optimal if and only if it satisfies the **KKT conditions**:
![[Pasted image 20240530164053.png]]
All these 5 equations must be satisfied!

We can compute the **scores** as:
$$
s(x_{t}) = \sum_{i=1}^n \alpha_{i} z_{i} x_{i}^T x_{i} +b
$$
Notice that, again, this depends only on dot products $x_{i}^T x_{i}$
The training points that are not Support Vectors do not affect the separation surface.

# SVM (no linearly separable classes)

Let’s consider a problem where classes are not linearly separable.
There always be a point that violates the constraint $z_{i}(w^Tx_{i} +b) \geq 1$.

We can try to minimize the number of points that violate the constraint
To do this, we introduce the slack variables $\xi_{i} \geq 0$, which represent how much a point is violating the constraint.
We will allow points to be inside the margin by a factor:
$$
z_{i}(w^Tx_{i} +b) \geq 1 - \xi_{i}
$$
We want to minimize the number of data points that lie inside the margin, i.e. the number of points for which ξi > 0:
$$
\Phi(\xi) = \sum_{i=1}^n \xi_{i}^\sigma
$$
For small values of $\sigma$ this function is the **number of points that are inside the margin**: if we remove those points we can then train a maximum margin hyperplane over the remaining points.
Formally we need to **minimize the following functional**:
![[Pasted image 20240530165506.png]]
Where $F$ is a function and $C$ a constant.
For sufficiently large $C$ and small $\sigma$ **the optimal solution minimizes the number of points that violate the margin constraints and maximizes the margin between the remaining points**.

Unfortunately, computation for small values of $\sigma$ is complex, thus we focus on SVM with $\sigma = 1$ (sometimes $\sigma=2$ is used, we call it L2-SVM): the new problem is the following
![[Pasted image 20240530165745.png]]

The term $\xi_{i}$ acts as a **penalty**:
- Points inside the margin have a value of xi > 0
- Points misclassified > 1
- $\sum_{i} \xi_{i}$ is an upper bound for misclassified point (errors)
- $C$ allows a trade off between margin and errors on the training set
This solution is called **soft margin**:

![[Pasted image 20240530170959.png]]

As we did before we can cast the Lagrangian problem:
![[Pasted image 20240530171138.png]]
For KKT we have the following conditions:
![[Pasted image 20240530171155.png]]

Solving the constraints and replacing we obtain the dual problem:

![[Pasted image 20240530171250.png]]

Scores can be computes same as hard margin version.

# Solving the primal problem

For data points correctly classified we have $\xi_{i}=0$, while for misclassified we have: $\xi_{i} = 1 -z_{i}(w^Tx_{i} +b)$.
The primal problem can be written as:
$$
	\min_{w,b} \frac{1}{2} \lvert  \lvert w \rvert  \rvert^2 + C \sum_{i} \max [0, 1-z_{i}(w^Tx_{i}+b)] 
$$
where the constraints have been absorbed by the **loss term**: $\max[0, 1-z_{i}(w^Tx_{i}+b)]$

This loss function is called **hinge loss** also denoted as:
$$
f(s) =\max (0, 1-s)
$$
We can rewrite the primal problem:
$$
	\min_{w,b} \frac{\lambda}{2} \lvert  \lvert w \rvert  \rvert^2 + \frac{1}{n}\sum_{i} \max [0, 1-z_{i}(w^Tx_{i}+b)] 
$$

![[Pasted image 20240530172038.png]]
![[Pasted image 20240530172110.png]]

# Summary

We have seen that we can obtain the separation hyperplane by solving either the primal or the dual problem:
![[Pasted image 20240530172146.png]]
![[Pasted image 20240530172233.png]]

# Kernel

Since we need to compute dot products, it would be nice to have a function that computes it efficiently  $k(x_{1},x_{2})$. In that case we only need $k$ to train our model.
This function $k$ is called **kernel function**, ant it allows to train our SVM in a large dimensional **Hilbert space** $\mathcal{H}$ without explicit mapping.
Even if the expansion was finite, the complexity of the primal problem may be too large
On the other hand, the complexity of the dual problem only de pends on the number of training points.
In practice, we are computing a linear separation surface in the expanded space, which corresponds to a non–linear separation surface in the original feature space.

