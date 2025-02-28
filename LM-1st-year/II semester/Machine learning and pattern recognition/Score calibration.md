# Why Score calibration?

Some model's scores do not represent LLRs for the application, so decision based on a given threshold may be sub-optimal (miscalibration).
To avoid this problem we have two strategies:
- Use a validation (small) set to find a optimal threshold for a given application, but in order to do so we need to know the target application
- We can use **functions that transform scores into well calibrated LLRs**

# Score models

These are approximation to the isotonic regression transformation. example: prior-weighted logistic regression.
We consider non-calibrated scores as a 1-D array: we assume a mapping between non and calibrated scores:
$$
f(s) = \alpha s + \gamma
$$
Since $f(s)$ should be well calibrated, it must be considered to be a log-likelihood ratio:
$$
\log  \frac{P(C=H_{T}| s)}{P(C=H_{F}|s)} = \alpha s + \gamma + \log \frac{\tilde{\pi}}{1- \tilde{\pi}} = \alpha s+\beta
$$

### Prior weighted logistic regression

We can use a model (such as prior weighted logistic regression) to learn the values of $\alpha,\beta$ from a set of samples, that we call **calibration training set**.
(In practice we treat scores as a 1-d feature vectors dataset)
Once we have estimated the parameters we can compute the calibrated score with:
$$
f(s) = \alpha s + \beta - \log \frac{\tilde{\pi}}{1- \tilde{\pi}}
$$
This approach needs a prior and gives calibrated scores for that specific prior and also a wider range of values close to the original one.

### Generative models

Alternatively we can use generative models: we can model the two scores based on the class $S|H_{T}, S|H_{F}$ then we can estimate the densities:
$$
f_{S|H_{T}}(s) = \mathcal{N}(s|\mu_{T}, \nu_{F}) \quad \quad  f_{S|H_{F}}(s) = \mathcal{N}(s|\mu_{F}, \nu_{F})
$$
Then we can compute the calibrated score as:
$$
f_{cal} (s) = \log \frac{f_{S|H_{T}(s)}}{f_{S|H_{F}(s)}}
$$
Often we use tied models: $\nu_{F} = \nu_{T} = \nu$ as this results in **monotonic transformation**

# Calibration set

Two possible scenarios:
- Miscalibration due to non-probabilistic scores, or to overfitting or underfitting models, but evaluation and training populations are similar: **the calibration set can be extracted from the training set material**
- Miscalibration due to mismatch between training and the application / evaluation population: the calibration set (and in this case the calibration validation set as well, if we want to assess the goodness of the calibration) should mimic the application population

So the new setup for ML project will be:

![[Pasted image 20240613154219.png]]

For simplicity we obtain our calibration split from our former validation set.
New problem: few data for those datasets.
We need to find a trade off because leaving a set with few data will lead to unreliable results
So solve this we can use **K-Fold**.

# K-Fold 

We split the dataset into K partitions (usually with the similar amount of samples per partition), keeping the same class ratios as the original set.
Iteratively we train a model using K-1 folds then we validate with the left-out fold:

![[Pasted image 20240613154808.png]]

Putting together the scores from all folds we can perform an evaluation.
Some **important notes**:
- All $K$ models must be trained with the same setup (same hyperparameters, same pre-processing)
- For metrics such as MinDCF we need to compute those metrics over the **pooled scores** (not over each fold, or we get biased results )

Final step is train one more model over the whole training set, using hyperparameters we estimated in previous set

### Number of K

One first option is **leave-one-out**: $K=N$ but is time consuming
We need a trade off:
- Bigger value of $K$: time consuming but more robust
- Smaller value: less robust but faster

### Extending the Approach to Three-Set Partitioning

We can extend our approach to three-set partitioning using a two-step method:

#### First Step: K-Fold for Training Classifier
1. **Train K Classifiers:** Each classifier is trained without one fold (fold k), creating models \( R_k \).
2. **Score Each Fold:** Each fold k is scored using model \( R_k \).
3. **Train a General Classifier:** A classifier \( R_F \) is trained over the entire training set for later use.
4. **Pool Scores for Calibration:** Scores from each fold are pooled together to form a calibration set.

#### Second Step: K-Fold for Calibration Scores
1. **Apply K-Fold on Calibration Scores:** A different randomized shuffle is used.
2. **Train K Calibration Models:** Calibration models \( C_k \) are trained.
3. **Train a General Calibration Model:** Model \( C_F \) is trained over all pooled scores for later use.
4. **Calibrate Scores of Each Fold:** Each fold k is calibrated with model \( C_k \).
5. **Evaluate Model Performance:** The pooled calibrated scores are evaluated to choose the best configuration.
6. **Classify Application Samples:** Use classification model \( R_F \) followed by calibration model \( C_F \).

### Course Project Simplification
- Use a simple split for model training.
- In the final lab, apply K-Fold for calibration only.
- Estimate a calibration model from the validation set scores, split them into K-folds, train K calibration models, score K folds, pool the scores, and evaluate the calibration effectiveness.

### Score-Level Fusion

#### Combining Competing Models
- Competing models with similar or differing performance can extract different information.
- Combining these models can enhance classification performance for test samples.

#### Simple Fusion Strategies
- **Majority Voting:** Each model's prediction is considered, and the most selected label is assigned. Limitations include the need for tie-breaking rules and not accounting for the strength of each classifier's contribution.

#### Example of Voting Limitation
- With three classifiers giving scores \( s1 = 10.0 \), \( s2 = -0.1 \), \( s3 = -0.1 \) (binary task), majority voting may incorrectly label the sample due to two uncertain systems despite one confident one.

#### Combining Scores
- **Average or Sum of Scores:** Simple methods like averaging or summing scores can be used. However, if models use correlated features, the sum may not be accurate.

#### Weighting Contributions
- **Weighted Scores:** Scores can be combined using weights, with the fused score \( s_{\text{fused}} \) calculated as \( s_{\text{fused}} = \alpha_1 s1 + \alpha_2 s2 + \alpha_3 s3 + \cdots + \alpha_m s_m + \gamma \).
- **Estimating Weights:** Weights \( \alpha_i \) and bias \( \gamma \) can be estimated, often using logistic regression.

#### Final Score Fusion
- **Affine Transformation:** The fused score is an affine transformation of the score vector, and logistic regression can estimate the weights and bias.
- **Calibration Partition:** The calibration partition is used to estimate score fusion weights, akin to the calibration procedure for a single system.
