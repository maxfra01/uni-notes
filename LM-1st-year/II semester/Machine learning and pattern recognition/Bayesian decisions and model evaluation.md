# Model evaluation

Up to now we have assigned labels according to a maximum-a-posterior probability. For example let's consider classifying a patient as healthy or ill: the impact of a bad decision can lead to injuries of the patient.
So, assigning labels on a maximum-a-posterior probability **does not consider the fact that different labels can have different impact**.

We already defined two metrics to measure the correctness of a model: **accuracy** and **error rate**:
$$
\text{error rate} = \frac{\text{\# of incorrect classified samples}}{\text{\# of samples}} \quad \quad \text{accuracy} = 1 - \text{error rate}
$$
These approaches do not consider the impact of errors, they depend of a-priori probability and are **unnormalized**.
We cannot see how a model performs on application data, because generally we don't have neither application data and labels.
We can employ an **evaluation set** to compute the performance of different models on the evaluation set itself: this performance can be used to **estimate the performance on application data**, that behave similarly to the evaluation set.

First we need to define the **confusion matrix**: for each class $C_{i}$ and predicted class $C_{j}$ the matrix collects the number of samples of $C_{i}$ predicted as $C_{j}$:

![[Pasted image 20240502151605.png]]

Note that the diagonal elements are the correctly classified.
For a **binary problem** the confusion matrix is composed by **true and false negative** and **true and false positive**.
$$
\text{acc} = \frac{TP + TN}{TP + TN + FP + FN} \quad \quad \text{err}= \frac{FP + FN}{TP + TN + FP + FN}
$$
However this metrics ignores that application data can have different a priori probability: we need to define **per-class error rates**:

| Per-class error rates                                    | Description                  |
| -------------------------------------------------------- | ---------------------------- |
| False negative rate FNR, (error rate for positive class) | $P_{fn}=\frac{FN}{FN + TP}$  |
| False positive rate FPR, error rate for negative class   | $P_{fp}= \frac{FP}{FP + TN}$ |
| True positive rate TPR (**recall**, sensitivity)         | $\frac{TP}{FN  TP} = 1 -FNR$ |
| True negative rate TNR (specificity)                     | $\frac{TN}{FP+TN}= 1-FPR$    |
These metrics are **unaffected by the proportion** of samples of each class.

We can define the **empirical prior probability** which corresponds to the fraction of positive samples:
$$
\pi_{\epsilon}^{\text{emp}} = \frac{TP + FN}{TP + TN + FP + FN}
$$
Then we can rewrite the **error rate** as a weighted sum of the error rates of each class, where
the weights are the empirical class priors of the dataset:
$$
\text{err} = P_{fp}(1 - \pi_{\epsilon}^\text{emp}) + P_{fn} \pi_{\epsilon}^\text{emp}
$$
if the **prior probability of being positive is low**, than the first recognizer results in a
lower total number of errors. However, it will incorrectly label as negative more positive patients.
We can imagine that incorrectly labeling as negative a positive patient may be more dangerous than the opposite, i.e., it may **have a larger cost**.

# Bayes Risk

Consider the core definition of a classification problem.
Given a task, the goal of a classifier is to perform an operation $a$, chosen from a set of operations $\mathcal{A}$ (for example, assign label $a$ to a sample).
We can **associate to each action a cost $\mathcal{C}(a | k)$** that we have to pay when we choose action $a$ and the sample belongs to class $k$.

(in the following scenario, we assume that a classifier is able to produce a decision according to some rule)

We denote with $a(x, \mathcal{R})$ the decision made by model $\mathcal{R}$ for sample $x$, whose correct class label $c$ : the cost of such decision is $\mathcal{C(a(x, \mathcal{R})|c)}$.
If we are considering class labeling, this above is the cost of predicting label $a(x, \mathcal{R})$ when the correct class is $c$.

We can **define the bayes risk** which is the expected cost we have to pay using our system on the application population $\epsilon$.
$$
\mathcal{B} = \mathbb{E}_{X,C | \epsilon}[\mathcal{C}(a(x, \mathcal{R})| c)]
$$
The application population $\epsilon$ can also be interpreted as an **evaluator** who has complete knowledge on application data.
To compute this expected value we need the distribution of $X, C| \epsilon$ but it is **unknown**.
So we suppose that **we have knowledge about prior probability** $\pi_{c}$.
The distribution of $X,C|\epsilon$, as we said before, is unknown but we can compute, through the evaluation set , the **average cost on all samples** belonging to evaluations set.
Combining all together we can compute the **empirical Bayes risk** as:
$$
\mathcal{B}_{\text{emp}} = \sum_{c=1}^K \frac{\pi_{c}}{N_{c}} \sum_{i|c=c} \mathcal{C}(a(x_{i}, \mathcal{R})|c )
$$
The risk **measures the costs of our decisions** for a target application over the **evaluation** **samples**.
Using $\mathcal{B}_{\text{emp}}$ we can compare recognizers: lower risks corresponds to better performances.

### Compute risk from confusion matrix

Let's consider a 3 class problem with the following data:
$$
C = \begin{bmatrix}
0 & 1 & 2 \\
1 & 0 & 1 \\
2 & 1 & 0
\end{bmatrix} \quad \quad \pi = \begin{bmatrix}
0.3 \\
0.4 \\
0.3
\end{bmatrix} \quad \quad M = \begin{bmatrix}
205 & 111 & 56 \\
145 & 199 & 121 \\
50 & 92 & 225
\end{bmatrix}
$$
Now for each class we compute:
$$
\frac{\pi_{c}}{N_{c}} \sum_{i|c=c} \mathcal{C}(a(x_{i}, \mathcal{R})|c )
$$
Consider for example class 1 (the first column): $N_{1}=205+145+50=400$, $\pi_{1}=0.3$ For samples that are correctly classified (205) the cost is 0; for samples that are classified as class 2 (145) the cost is 1; for samples that are classified as class 3 (50) the cost is 2.
$$
\frac{\pi_{1}}{N_{1}} \sum_{i|c=1} \mathcal{C}(a(x_{i}, \mathcal{R})|c ) = \frac{0.3}{400}(0 \cdot 205 + 1\cdot 145 + 2 \cdot 50) = 0.18375
$$
Similar for class 2 and 3, **summing** all values and we obtain the $\mathcal{B}_{\text{emp}}$ risk.

### Compute risk from confusion matrix for a binary problem

Nothing change, but now costs are called $C_{fp}$ and $C_{fn}$:

![[Pasted image 20240509113003.png]]

We proceed to the calculation of the Bayes risk and we obtain (only for binary classification):
$$
\mathcal{B}_{\text{emp}} = \text{DCF}_{\text{un-normalized}} =  \pi_{T} C_{fn} P_{fn} + (1-\pi_{T})C_{fp}P_{fp}
$$
This expression above is also called **Detection cost function** and has the following characteristics:
- It describes the two possibile costs of errors ($C_{fp}, C_{fn}$)
- It defines the class prior probability ($\pi_{T}$)
- It evaluates our result by computing the Bayes risk
- It describes the **positive and negative ratios**, which depends by a threshold $t$.

In order to **normalized the DCF** we need to perform the following operation:
$$
\frac{\text{DCF}(\pi_{T}, C_{fp}, C_{fn})}{\min(\pi_{T}C_{fn}, (1-\pi_{T}C_{fp}))}
$$
We can also normalize DCF using the **effective prior**: if we compute the effective prior and consider an application with $C_{fn}=1$ and $C_{fp}=1$ we will have the same result.
The effective prior is equal to:
$$
\tilde{\pi} = \frac{\pi_{T}C_{fn}}{\pi_{T}C_{fn} + (1-\pi_{T})C_{fp}}
$$
# Visualizing error rate

For binary classification problem, since now, we have computed a log-likelihood ratio and then compare it with a log prior-odd (a threshold). These are called **generative model**.
We notice that at different thresholds correspond different error rates:

![[Pasted image 20240510132007.png]]

We can visualize the performance of the model by plotting the **error rate as a function of the threshold**. Also we can define a new metric: **Equal error rate** (EER) corresponding to a threshold where $FPR = FNR$.
We can also visualize directly the trade-off for different kinds of error:

![[Pasted image 20240510132837.png]]

Better models will have a ROC curve that squash in the left upper corner!

# Bayes decision

Since the error rates depend on the selected threshold, we would like to optimize the threshold selection for a given application: this means we want to transform the classifier scores in **effective decisions**.
For a moment let's suppose our classifier is **probabilistic** (is able to provide class posterior probability).

We can compute the expected cost of action $a$ according to posterior probability $P(C=k | x, \mathcal{R})$: we obtain $\mathcal{C_{x, \mathcal{R}}(a )}$ 
$$
\mathcal{C}_{x, \mathcal{R}}(a) = \sum_{k=1}^K C(a|k) \cdot P(C=k|x, \mathcal{R})
$$
The **Optimal Bayed decision consist in choosing the action $a^*(x,\mathcal{R})$ that minimize the expected cost**:
$$
 a^*(x,\mathcal{R}) = \arg\min_{a} \mathcal{C}_{x,\mathcal{R}}(a)
$$
Different recognizes will have different beliefs, and thus provide different decisions.

### Bayes risk and Bayes decision

Optimal Bayes decisions are **optimal from the point of view of the recognizer**.
If the evaluator and the recognized agree (provide the same posterior probability for any sample) then the Bayes decisions **minimize the Bayes risk**.

### Binary problem

Consider the classic confusion matrix with $C_{fn}$ and $C_{fp}$.
How can we assign a sample to one class? We can express the cost of our actions:
$$
H_{T} \to C_{fp} \cdot P(H_{F} | x, \mathcal{R}) \quad \quad H_{F} \to C_{fn} \cdot P(H_{T} | x, \mathcal{ R})
$$
I can assign a label by **comparing the cost of the two decision**: if the cost of predicting TRUE is lower than predicting FALSE, then i will choose TRUE (and viceversa).
When costs are equal we can choose whatever we want (cost will always be optimal for the bayes optimal decisions). We can also write the threshold as:
$$
r(x) =  \log \frac{C_{fn}P(H_{T}|x, \mathcal{R})}{C_{fp} P(H_{F}|x, \mathcal{R})}
$$
In case of $\mathcal{R}$ being a **generative model**, we can express $r(x)$ as a combination of log-likelihood, prior and costs:

![[Pasted image 20240510141827.png]]

Consider now the triplet $(\pi_{T}, C_{fn}, C_{fp})$, which is called **working point of an application**.
This notation is **redundant**, because this can be rewrite as $(\tilde{\pi}, 1, 1)$ using the **effective prior** described above.
Then, after rewriting, the threshold become:
$$
t = -\log \frac{\tilde{\pi}}{1- \tilde{\pi}}
$$
In real case conditions (non-accurate model assumptions, mismatch between train and test data...) we say that score are **mis-calibrated** so this threshold above is not optimal anymore.
We need to define new metrics:
- We can compute the $\text{minDCF}$ corresponding to the use of optimal threshold for a given evaluation set: this is the **cost we would pay if we knew before-hand the optimal threshold for the evaluation set**
- We can also compute the actual DCF obtained using the threshold corresponding to the effective prior $\tilde{\pi}$. The difference between the actual and minimum DCF represents the **loss due to score mis-calibration**.

We can **compare different systems** over different applications through **Bayes Error plots** which is a DCF plot as function of $\log \frac{\tilde{\pi}}{1- \tilde{\pi}}$

![[Pasted image 20240513225843.png]]

We can see that, in case of **calibrated score** the difference between actual and minimum DCF is close to zero.
Thus, we need a technique to **calibrate the scores**.

# Score calibration

We can consider the non-calibrated scores as features: we assume a linear mapping from non-calibrated scores to calibrated scores.
$$
f(s) = \alpha s + \gamma
$$
Since $f(s)$ should output well-calibrated scores, it can be interpreted as log likelihood ratio for the two class hypothesis:
$$
f(s) = \alpha s + \gamma = \log \frac{f_{S|C}(s| \mathcal{H}_{T})}{f_{S|C}(s|\mathcal{H}_{F})}
$$
Since we are working with prior $\tilde{\pi}$ this corresponds to:
$$
\log \frac{P(C=\mathcal{H}_{T}|s)}{P(C=\mathcal{H}_{F}|s)} = \alpha s + \gamma + \log \frac{\tilde{\pi}}{1-\tilde{\pi}} = \alpha s + \beta
$$
(we will refer to the set of scores as **calibration** set in the following)
To recover the calibrated score $f(s)$ we need to compute:
$$
f(s) = \alpha s+ \gamma = \alpha s + \beta - \log \frac{\tilde{\pi}}{1- \tilde{\pi}}
$$
Even if we are choose one single value of $\tilde{ \pi}$ the model will often provide good calibration for a wider range of different applications (i.e. different values of $\tilde{\pi}$).

Typically, two scenarios:
- Mis-calibration due to non-probabilistic scores, or to overfitting or underfitting models: the calibration set can be taken from the training set material
- Mis-calibration due to mismatched between training and evaluation population: the calibration set should mimic the evaluation population

For the project we are in the first scenario: the calibration set must be taken from the validation set (the evaluation set cannot be used to estimate any part of the model)
With a single-split approach we require 3 splits:
- Model training
- Calibration set
- Actual validation set
To use a 2-step procedure we will use **K-Fold**.

### K-Fold

