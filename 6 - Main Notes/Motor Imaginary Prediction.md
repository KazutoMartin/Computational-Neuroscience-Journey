2026-06-21 01:37 

Tags: [[BCI]] [[Linear Algebra]] [[Neurophysiology]]


# Motor Imaginary Prediction
## Math(CSP: Common spatial problem)
### Covariance Matrix
Let $X \in \mathbb{R}^{N \times T}$ be the EEG data matrix for a single trial, where:

- $N$ is the number of channels (electrodes).
- $T$ is the number of time samples.

We assume $X$ has been bandpass-filtered (usually between 8–30 Hz, the Mu and Beta bands where motor imagery occurs) and centered to have a mean of 0.


For each class (Class 1: Left Hand, Class 2: Right Hand), we calculate the normalized spatial covariance matrix $\Sigma$:

$$\Sigma_1 = \frac{X_1 X_1^T}{\text{Tr}(X_1 X_1^T)} \qquad \Sigma_2 = \frac{X_2 X_2^T}{\text{Tr}(X_2 X_2^T)}$$

Where $X^T$ is the transpose, and $\text{Tr}(\cdot)$ is the trace (the sum of the diagonal elements), which normalizes for total signal power across trials. We average these covariance matrices over all training trials for Class 1 and Class 2 to get $\bar{\Sigma}_1$ and $\bar{\Sigma}_2$.

> The diagonal in $X_1 X_1^T$ is the sum of squared value of each channel in time, which gives us the variance of that channel. 

> The off-diagonal is the dot product of 2 channel data, which measures alignment(like cosine-similarity). The off-diagonal of $X_1 X_1^T$ is covariance between channels

> 	We normalize $X_1 X_1^T$ by dividing it by its trace to ensure the matrix elements are scaled relative to the total signal power(variance). Because the trace of a covariance matrix represents the total variance (and therefore the total power) of the multi-channel EEG signal, this normalization controls for cross-trial amplitude variations. This is particularly crucial in EEG processing, where non-neural artifacts can artificially inflate the absolute signal power, potentially biasing subsequent analysis.


We average these covariance matrices over all training trials for Class 1 and Class 2 to get $\bar{\Sigma}_1$ and $\bar{\Sigma}_2$.

### Optimization
We want to find a spatial filter vector $w \in \mathbb{R}^N$ that projects the multi-channel EEG data into a single time series $y = w^T X$.

The variance of this projected signal is: $$w^T X (w^T X)^T = w^T X X^T w = w^T \Sigma w$$We want to maximize this variance for Class 1 relative to Class 2. This is formulated using the Rayleigh quotient:

$$\max_{w} J(w) = \frac{w^T \bar{\Sigma}_1 w}{w^T \bar{\Sigma}_2 w}$$
This can be solved by GEVD.
#### Generalized Eigenvalue decomposition
In a standard eigenvalue problem, you solve for a scalar $\lambda$ and a non-zero vector $v$ such that:

$$Av = \lambda v$$

In the **generalized eigenvalue problem**, a second square matrix $B$ is introduced on the right side:

$$Av = \lambda Bv$$
If $B$ is the identity matrix ($B = I$), the problem immediately collapses back into the standard eigenvalue problem.

#### Lagrange Multipliers
If we just try to maximize the numerator, $w$ could grow infinitely large. To prevent this, we constrain the denominator to be a constant value, typically 1. This turns our problem into a constrained optimization problem:

$$\max_{w} w^T \bar{\Sigma}_1 w \quad \text{subject to} \quad w^T \bar{\Sigma}_2 w = 1$$
To solve this, we use the method of **Lagrange Multipliers**. We set up the Lagrangian function $\mathcal{L}(w, \lambda)$ by subtracting the constraint (multiplied by a scalar $\lambda$) from our objective function:

$$\mathcal{L}(w, \lambda) = \underbrace{w^T \bar{\Sigma}_1 w}_{\text{Objective to Maximize}} - \underbrace{\lambda (w^T \bar{\Sigma}_2 w - 1)}_{\text{Penalty for Violating Constraint}}$$

To find the maximum, we take the derivative of $\mathcal{L}$ with respect to the vector $w$ and set it to zero. Using matrix calculus rules ($\frac{\partial}{\partial w}(w^T A w) = 2Aw$ for a symmetric matrix $A$):

$$\frac{\partial \mathcal{L}}{\partial w} = 2\bar{\Sigma}_1 w - 2\lambda \bar{\Sigma}_2 w = 0$$

Divide by 2 and move the second term to the right side:

$$\bar{\Sigma}_1 w = \lambda \bar{\Sigma}_2 w$$
This is the exact definition of the **Generalized Eigenvalue Decomposition (GEVD)**.

When you solve this equation, you get a set of spatial filters (eigenvectors $w$) and their corresponding ratios (eigenvalues $\lambda$).

- If $\lambda = 9$, it means for that specific spatial filter $w$, the variance of Class 1 is **9 times higher** than the variance of Class 2.
    
- If $\lambda = 0.1$, it means the variance of Class 1 is only **one-tenth** of Class 2 (meaning Class 2 has much higher variance here).
    

By choosing the filters corresponding to the absolute largest eigenvalues and the absolute smallest eigenvalues, we get the best possible spatial filters to separate our two neural states.

#### What exactly does $w$ do?
It looks at the data from a point of view which has the most variance for the target class. Just like we are pointing a microphone to one of the crowds in a stadium.
When you apply this filter ($y = w^T X$), you are combining the channels in a way that causes the sound waves of Speaker 2 to destructively interfere and cancel out completely, while the sound waves of Speaker 1 constructively interfere and ring out loudly.
![[Figure_1.png]]
### Picking the best 2m vectors
When you solve this GEVD, you get a matrix of eigenvectors $W = [w_1, w_2, \dots, w_N]$ and a diagonal matrix of eigenvalues $\Lambda$.

Because of the way the problem is structured, the eigenvalues $\lambda$ represent the ratio of variance between Class 1 and Class 2:

- An eigenvalue near **maximum** means that the corresponding spatial filter $w$ yields very **high variance for Class 1** and **low variance for Class 2**.
    
- An eigenvalue near **minimum** (close to 0) means the spatial filter yields very **low variance for Class 1** and **high variance for Class 2**.
    

By choosing the first $m$ and the last $m$ eigenvectors (typically $m=2$ or $3$), we form a compact spatial filter matrix $W_{\text{reduced}} \in \mathbb{R}^{2m \times N}$.

When we multiply our raw data by this matrix:

$$Z = W_{\text{reduced}} X$$

The rows of $Z$ are the maximally discriminative source signals. To feed this into a machine learning classifier (like an SVM or LDA), we simply compute the log-variance of these $2m$ rows.

### Why we pick 2m of these and not just the maximum one?
1. We do it to be considerate about the noise.
2. Remember that BCI binary classification is a two-way street.

- **The Max Eigenvalue Filter ($w_1$)** is a specialist at finding where **Class 1 is loud and Class 2 is quiet**.
    
- **The Min Eigenvalue Filter ($w_N$)** is a specialist at finding where **Class 2 is loud and Class 1 is quiet**.
    

If you only use the maximum eigenvalue filter, you are only looking at the brain through a lens optimized for Class 1.

Imagine a trial comes in where the subject gets distracted, loses focus, or blinks, and the overall brain activity drops across the board.

- If you pass this dirty data through _only_ the Max Filter, the variance will be **low**.
    
- Because the variance is low, your classifier will think: _"Ah! The variance isn't high, so it must not be Class 1. Therefore, it must be Class 2!"_
    

By using **both** the max and min filters, you give your classifier a two-dimensional coordinate system to verify the state:

```
                  Filter_Min (Class 2 Specialist)
                        ^
                        |     [Class 2 Trials Cluster Here]
                        |     (Low Class 1, High Class 2)
                        |
                        |
   ---------------------+---------------------> Filter_Max (Class 1 Specialist)
                        |
                        |
[Class 1 Trials Cluster Here]
(High Class 1, Low Class 2)
                        |
```

If a trial has low variance on _both_ filters (collapsing near the origin `[0,0]`), the classifier knows it is just noise or a dead trial, rather than falsely misclassifying it as Class 2. It forces the machine learning model to see a true push-and-pull relationship.
# References