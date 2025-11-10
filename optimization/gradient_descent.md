# Gradient Descent

## Overview

Gradient Descent is an iterative optimization algorithm used to find the minimum of a function. It's one of the most fundamental algorithms in machine learning and is used to minimize the cost function during model training.

## Mathematical Foundation

### Basic Update Rule

For a function `f(x)`, the gradient descent update rule is:

```
x_{t+1} = x_t - α ∇f(x_t)
```

Where:
- `x_t` is the parameter at iteration t
- `α` is the learning rate (step size)
- `∇f(x_t)` is the gradient of the function at x_t

### Gradient

The gradient is a vector of partial derivatives:

```
∇f(x) = [∂f/∂x₁, ∂f/∂x₂, ..., ∂f/∂xₙ]ᵀ
```

## Algorithm Steps

1. **Initialize** parameters randomly or with predefined values
2. **Calculate** the gradient of the cost function
3. **Update** parameters in the opposite direction of the gradient
4. **Repeat** steps 2-3 until convergence or max iterations

## Types of Gradient Descent

### 1. Batch Gradient Descent
- Uses entire dataset to compute gradient
- More stable but slower for large datasets
- Guaranteed to converge to global minimum for convex functions

### 2. Stochastic Gradient Descent (SGD)
- Uses one sample at a time
- Faster but noisier updates
- Can escape local minima due to noise

### 3. Mini-batch Gradient Descent
- Uses a small batch of samples
- Balances stability and speed
- Most commonly used in practice

## Learning Rate

The learning rate α is critical:
- **Too large**: Algorithm may overshoot and diverge
- **Too small**: Convergence is very slow
- **Optimal**: Fast convergence to minimum

### Learning Rate Schedules
- Step decay
- Exponential decay
- Adaptive learning rates (Adam, RMSProp)

## Convergence Criteria

Stop when:
- Change in cost function < threshold
- Gradient magnitude < threshold
- Maximum iterations reached
- Validation performance stops improving

## Practical Considerations

1. **Feature Scaling**: Normalize features to similar ranges
2. **Initialization**: Good initial values can speed up convergence
3. **Monitoring**: Track cost function to detect divergence
4. **Early Stopping**: Prevent overfitting in ML models

## Python Implementation Example

```python
import numpy as np

def gradient_descent(gradient_func, x_init, learning_rate=0.01, 
                     max_iterations=1000, tolerance=1e-6):
    """
    Basic gradient descent implementation
    
    Args:
        gradient_func: Function that computes gradient
        x_init: Initial parameter values
        learning_rate: Step size
        max_iterations: Maximum number of iterations
        tolerance: Convergence threshold
    
    Returns:
        Optimized parameters
    """
    x = x_init
    
    for i in range(max_iterations):
        gradient = gradient_func(x)
        x_new = x - learning_rate * gradient
        
        # Check convergence
        if np.linalg.norm(x_new - x) < tolerance:
            print(f"Converged after {i+1} iterations")
            break
            
        x = x_new
    
    return x

# Example: Minimize f(x) = x^2
def gradient_squared(x):
    return 2 * x

# Run optimization
result = gradient_descent(gradient_squared, x_init=10.0, learning_rate=0.1)
print(f"Minimum found at x = {result}")
```

## References

- [Gradient Descent - Wikipedia](https://en.wikipedia.org/wiki/Gradient_descent)
- Deep Learning Book by Goodfellow et al.
- Pattern Recognition and Machine Learning by Bishop
