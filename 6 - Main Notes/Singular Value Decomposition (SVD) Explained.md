2026-06-04 00:03 

Tags: [[SVD]]


# Singular Value Decomposition (SVD)
It works for all matrixes($\lambda_i$ in $A^T.A$ are always non-negative). That's why it is so powerful.
## The Mathematical Definition
$$A = U \Sigma V^T$$
## The Geometric Intuition

Any matrix AAA acts as a linear transformation on space (like stretching, squishing, or rotating a 2D grid or 3D space). SVD tells us that **any** linear transformation, no matter how weird, can be broken down into three simple steps:

1. **$V^T$(Rotation/Reflection):** First, the space is rotated or reflected.
2. **$\Sigma$(Scaling):** Second, the space is stretched or shrunk along the standard coordinate axes. The singular values dictate exactly how much stretching or shrinking happens.
3. **$U$(Rotation/Reflection):** Finally, the space is rotated or reflected again into its final position.
# References

[[Singular Value Decomposition (SVD) Conceptually Explained]]

[[Singular Value Decomposition (SVD) Calculating Explained]]