2026-06-09 02:06 

Tags: [[calculus]]


# Gradient Descent

It's taking the derivative and moving in that direction so we find the local optimized point, a local minimum point. For larger dimensions, we use gradients, and that will guide us into the local minimum point. We use a learning rate to normalize the effect of the gradient, the amount of the gradient. If we set it too high, it will overshoot, and if we set it too low, it will take forever to reach the minimum, so we better optimize it.
A visual explanation of what it looks like can be found in the video referenced below. In summary, it is like a ball falling from a steep mountain and finding its way toward the minimum point of that mountain, but this example is in 3D. For higher dimensions, we can't really visualize the procedure, but the calculus and the calculations stay the same. 
Because we can't find the global minimum point with this algorithm, we will do it for random points. It is like we put balls on random points in the mountain and let them fall off and find their main point, and we use that as our global optimized point. So this method does not give us the global minimum or optimized point.
For example, this method is beneficial in neural networks. we have a cost function and we want to make that minimum with the weights and biases that we have. I don't know much about that at this point, but I know this algorithm or method is used in that field. 
# References

[[Gradient Descent Videos]]