# Linear Regression

## Overview

Linear Regression is a supervised learning algorithm used to predict a continuous output variable based on one or more input features. It assumes a linear relationship between input features and the output.

## Mathematical Foundation

### Simple Linear Regression

For a single feature:

```
y = wx + b
```

Where:
- `y` is the predicted output
- `x` is the input feature
- `w` is the weight (slope)
- `b` is the bias (intercept)

### Multiple Linear Regression

For multiple features:

```
y = w₁x₁ + w₂x₂ + ... + wₙxₙ + b
```

Or in vector form:

```
y = wᵀx + b
```

### Matrix Form

For multiple samples:

```
Y = Xw + b
```

Where:
- `Y` is the vector of outputs (n × 1)
- `X` is the feature matrix (n × m)
- `w` is the weight vector (m × 1)
- `b` is the bias term

## Cost Function

### Mean Squared Error (MSE)

The most common cost function for linear regression:

```
J(w, b) = (1/2m) Σᵢ₌₁ᵐ (ŷᵢ - yᵢ)²
```

Where:
- `m` is the number of training examples
- `ŷᵢ` is the predicted value
- `yᵢ` is the actual value

## Optimization Methods

### 1. Gradient Descent

Iteratively update weights using:

```
w := w - α (∂J/∂w)
b := b - α (∂J/∂b)
```

Gradients:
```
∂J/∂w = (1/m) Xᵀ(Xw - y)
∂J/∂b = (1/m) Σ(ŷᵢ - yᵢ)
```

### 2. Normal Equation (Closed-form Solution)

Direct calculation without iteration:

```
w = (XᵀX)⁻¹Xᵀy
```

**Pros**: No need to choose learning rate, no iterations
**Cons**: Slow for large number of features (O(n³) complexity)

## Assumptions

1. **Linearity**: Relationship between X and y is linear
2. **Independence**: Observations are independent
3. **Homoscedasticity**: Constant variance of errors
4. **Normality**: Errors are normally distributed
5. **No multicollinearity**: Features are not highly correlated

## Evaluation Metrics

### R² Score (Coefficient of Determination)

```
R² = 1 - (SS_res / SS_tot)
```

Where:
- `SS_res` = Σ(yᵢ - ŷᵢ)² (residual sum of squares)
- `SS_tot` = Σ(yᵢ - ȳ)² (total sum of squares)

### Mean Absolute Error (MAE)

```
MAE = (1/m) Σ|yᵢ - ŷᵢ|
```

### Root Mean Squared Error (RMSE)

```
RMSE = √[(1/m) Σ(yᵢ - ŷᵢ)²]
```

## Regularization

To prevent overfitting:

### Ridge Regression (L2)

```
J(w) = MSE + λΣwᵢ²
```

### Lasso Regression (L1)

```
J(w) = MSE + λΣ|wᵢ|
```

### Elastic Net

```
J(w) = MSE + λ₁Σ|wᵢ| + λ₂Σwᵢ²
```

## Python Implementation Example

```python
import numpy as np

class LinearRegression:
    def __init__(self, learning_rate=0.01, iterations=1000):
        self.lr = learning_rate
        self.iterations = iterations
        self.weights = None
        self.bias = None
    
    def fit(self, X, y):
        """Train the model using gradient descent"""
        n_samples, n_features = X.shape
        
        # Initialize parameters
        self.weights = np.zeros(n_features)
        self.bias = 0
        
        # Gradient descent
        for _ in range(self.iterations):
            # Forward pass
            y_pred = np.dot(X, self.weights) + self.bias
            
            # Compute gradients
            dw = (1/n_samples) * np.dot(X.T, (y_pred - y))
            db = (1/n_samples) * np.sum(y_pred - y)
            
            # Update parameters
            self.weights -= self.lr * dw
            self.bias -= self.lr * db
    
    def predict(self, X):
        """Make predictions"""
        return np.dot(X, self.weights) + self.bias
    
    def mse(self, y_true, y_pred):
        """Calculate mean squared error"""
        return np.mean((y_true - y_pred) ** 2)

# Example usage
if __name__ == "__main__":
    # Generate sample data
    np.random.seed(42)
    X = 2 * np.random.rand(100, 1)
    y = 4 + 3 * X + np.random.randn(100, 1)
    
    # Train model
    model = LinearRegression(learning_rate=0.01, iterations=1000)
    model.fit(X, y.ravel())
    
    # Make predictions
    predictions = model.predict(X)
    
    print(f"Weights: {model.weights}")
    print(f"Bias: {model.bias}")
    print(f"MSE: {model.mse(y.ravel(), predictions)}")
```

## When to Use Linear Regression

**Good for:**
- Understanding feature relationships
- Prediction when relationship is approximately linear
- Baseline model for comparison
- Interpretable results needed

**Not ideal for:**
- Complex non-linear relationships
- When assumptions are violated
- High-dimensional data with limited samples

## References

- [Scikit-learn Linear Models](https://scikit-learn.org/stable/modules/linear_model.html)
- Introduction to Statistical Learning by James et al.
- The Elements of Statistical Learning by Hastie et al.
