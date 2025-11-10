# Matrix Operations

## Overview

Matrix operations are fundamental to linear algebra and are extensively used in machine learning, optimization, and data science. Understanding these operations is crucial for implementing and optimizing ML algorithms.

## Basic Matrix Definitions

### Matrix

A matrix is a rectangular array of numbers arranged in rows and columns:

```
A = [a₁₁  a₁₂  a₁₃]
    [a₂₁  a₂₂  a₂₃]
```

Dimensions: m × n (m rows, n columns)

### Vector

A vector is a matrix with one column (column vector) or one row (row vector):

```
v = [v₁]    (column vector: n × 1)
    [v₂]
    [v₃]
```

### Scalar

A single number (1 × 1 matrix)

## Matrix Operations

### 1. Addition and Subtraction

Matrices must have the same dimensions:

```
A + B = [a₁₁+b₁₁  a₁₂+b₁₂]
        [a₂₁+b₂₁  a₂₂+b₂₂]
```

**Properties:**
- Commutative: A + B = B + A
- Associative: (A + B) + C = A + (B + C)

### 2. Scalar Multiplication

Multiply each element by a scalar c:

```
cA = [ca₁₁  ca₁₂]
     [ca₂₁  ca₂₂]
```

### 3. Matrix Multiplication

For matrices A (m × n) and B (n × p):

```
C = AB (result is m × p)
c_ij = Σₖ₌₁ⁿ (aᵢₖ × bₖⱼ)
```

**Properties:**
- NOT commutative: AB ≠ BA (in general)
- Associative: (AB)C = A(BC)
- Distributive: A(B + C) = AB + AC

**Requirements:**
- Number of columns in A must equal number of rows in B

### 4. Transpose

Swap rows and columns:

```
If A = [1  2]  then Aᵀ = [1  3]
       [3  4]            [2  4]
```

**Properties:**
- (Aᵀ)ᵀ = A
- (AB)ᵀ = BᵀAᵀ
- (A + B)ᵀ = Aᵀ + Bᵀ

### 5. Dot Product

For vectors u and v:

```
u · v = Σᵢ uᵢvᵢ = u₁v₁ + u₂v₂ + ... + uₙvₙ
```

Or in matrix form: u · v = uᵀv

### 6. Element-wise (Hadamard) Product

Element-wise multiplication (denoted by ⊙):

```
A ⊙ B = [a₁₁b₁₁  a₁₂b₁₂]
        [a₂₁b₂₁  a₂₂b₂₂]
```

## Special Matrices

### Identity Matrix (I)

Square matrix with 1s on diagonal, 0s elsewhere:

```
I = [1  0  0]
    [0  1  0]
    [0  0  1]
```

Property: AI = IA = A

### Diagonal Matrix

Non-zero elements only on the diagonal:

```
D = [d₁  0   0 ]
    [0   d₂  0 ]
    [0   0   d₃]
```

### Zero Matrix

All elements are zero:

```
0 = [0  0]
    [0  0]
```

## Matrix Inverse

For square matrix A, if A⁻¹ exists:

```
AA⁻¹ = A⁻¹A = I
```

**Conditions for invertibility:**
- Matrix must be square (n × n)
- Determinant must be non-zero (det(A) ≠ 0)

**Properties:**
- (A⁻¹)⁻¹ = A
- (AB)⁻¹ = B⁻¹A⁻¹
- (Aᵀ)⁻¹ = (A⁻¹)ᵀ

## Determinant

For 2×2 matrix:

```
det(A) = |a  b| = ad - bc
         |c  d|
```

**Properties:**
- det(AB) = det(A) × det(B)
- det(Aᵀ) = det(A)
- det(A⁻¹) = 1/det(A)

## Trace

Sum of diagonal elements:

```
tr(A) = Σᵢ aᵢᵢ
```

**Properties:**
- tr(A + B) = tr(A) + tr(B)
- tr(cA) = c × tr(A)
- tr(AB) = tr(BA)

## Python Implementation Examples

```python
import numpy as np

# Create matrices
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

# Addition
C = A + B
print("A + B =\n", C)

# Matrix multiplication
D = np.dot(A, B)  # or A @ B
print("AB =\n", D)

# Element-wise multiplication
E = A * B
print("A ⊙ B =\n", E)

# Transpose
A_T = A.T
print("Aᵀ =\n", A_T)

# Inverse
A_inv = np.linalg.inv(A)
print("A⁻¹ =\n", A_inv)

# Verify inverse
I = np.dot(A, A_inv)
print("AA⁻¹ =\n", I)

# Determinant
det_A = np.linalg.det(A)
print("det(A) =", det_A)

# Trace
trace_A = np.trace(A)
print("tr(A) =", trace_A)

# Eigenvalues and eigenvectors
eigenvalues, eigenvectors = np.linalg.eig(A)
print("Eigenvalues:", eigenvalues)
print("Eigenvectors:\n", eigenvectors)
```

## Applications in Machine Learning

### 1. Linear Transformations
- Feature transformations
- Dimensionality reduction (PCA)

### 2. Neural Networks
- Forward propagation: y = Wx + b
- Backpropagation: gradient calculations

### 3. Least Squares
- Normal equation: w = (XᵀX)⁻¹Xᵀy

### 4. Covariance Matrices
- Statistical relationships between features
- Principal Component Analysis

### 5. Similarity Measures
- Cosine similarity: (uᵀv) / (||u|| ||v||)
- Distance metrics

## Important Concepts

### Matrix Rank
- Number of linearly independent rows or columns
- Full rank: rank = min(m, n)

### Orthogonal Matrices
- QᵀQ = QQᵀ = I
- Preserves lengths and angles

### Positive Definite Matrices
- xᵀAx > 0 for all x ≠ 0
- Important in optimization

## References

- Linear Algebra and Its Applications by Gilbert Strang
- [NumPy Linear Algebra](https://numpy.org/doc/stable/reference/routines.linalg.html)
- Deep Learning Book by Goodfellow et al. (Chapter 2)
