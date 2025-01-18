

# Comparison of Linear Regression, Lasso, Ridge, and Elastic Net

This document provides a detailed technical comparison of four commonly used regression techniques: **Linear Regression**, **Lasso Regression**, **Ridge Regression**, and **Elastic Net**. We will cover the mathematical formulations of each, how they differ, and the advantages and disadvantages of each method.

## 1. Linear Regression

### Overview
Linear Regression is the most basic and widely used method for predictive modeling. It assumes a linear relationship between the input variables (features) and the output variable (target). The goal is to minimize the difference between predicted and actual values, often measured by the **Mean Squared Error (MSE)**.

### Mathematical Formulation
In Linear Regression, we aim to fit a model of the form:

```
Y = X * β + ε
```

Where:
- `Y` is the target variable (size `n x 1`).
- `X` is the feature matrix (size `n x p`).
- `β` is the coefficient vector (size `p x 1`).
- `ε` is the error term (size `n x 1`).

The objective is to minimize the **Ordinary Least Squares (OLS)** cost function:

```
β̂ = argmin( ||Y - X * β||^2 )
```

The closed-form solution is:

```
β̂ = (X^T * X)^(-1) * X^T * Y
```

### Pros:
- Simple to implement and interpret.
- Efficient for small datasets with few features and low multicollinearity.
- Closed-form solution (no iterative optimization needed).

### Cons:
- Susceptible to **overfitting** when there are many features relative to the number of observations.
- Does not handle **multicollinearity** well (highly correlated features).
- Assumes homoscedasticity, normality of errors, and no multicollinearity, which may not always hold.

---

## 2. Ridge Regression

### Overview
Ridge Regression is an extension of Linear Regression that includes an **L2 penalty** (also known as **Tikhonov regularization**) to prevent overfitting by penalizing large coefficients.

### Mathematical Formulation
Ridge Regression modifies the Linear Regression cost function by adding a regularization term:

```
β̂ = argmin( ||Y - X * β||^2 + λ * ||β||^2 )
```

Where:
- `λ` is the regularization parameter that controls the strength of the penalty.
- `||β||^2` is the squared L2-norm of the coefficient vector.

The closed-form solution for Ridge Regression is:

```
β̂ = (X^T * X + λ * I)^(-1) * X^T * Y
```

Where `I` is the identity matrix.

### Key Differences from Linear Regression:
- Ridge penalizes large coefficients but does not eliminate them.
- It reduces the impact of multicollinearity by shrinking coefficients.

### Pros:
- Reduces overfitting when there are many features or correlations between predictors.
- More stable than Linear Regression in the presence of multicollinearity.
- Closed-form solution available.

### Cons:
- Does not perform feature selection (coefficients are not set to zero).
- Requires careful tuning of the regularization parameter `λ`.

---

## 3. Lasso Regression

### Overview
**Lasso Regression** (Least Absolute Shrinkage and Selection Operator) adds an **L1 penalty** to the cost function, which both shrinks coefficients and can set some coefficients exactly to zero, performing feature selection.

### Mathematical Formulation
Lasso modifies the cost function as follows:

```
β̂ = argmin( ||Y - X * β||^2 + λ * ||β||_1 )
```

Where:
- `||β||_1` is the **L1 norm** (sum of absolute values of the coefficients).

Unlike Ridge, which shrinks coefficients toward zero, Lasso encourages sparsity (some coefficients become exactly zero).

### Key Differences from Ridge:
- Lasso can **set some coefficients to exactly zero**, effectively performing feature selection.
- Lasso performs **shrinkage** (like Ridge) and **variable selection** (by removing features with zero coefficients).

### Pros:
- Automatic **feature selection**.
- Ideal for sparse models where only a subset of features are important.
- Useful when you have many predictors and expect only a few to have significant impacts on the outcome.

### Cons:
- Can be unstable when predictors are highly correlated.
- The choice of regularization parameter `λ` is critical, requiring cross-validation.

---

## 4. Elastic Net

### Overview
**Elastic Net** is a hybrid method that combines the penalties of both Ridge and Lasso regression. It is useful when there are many correlated features and you want the benefits of both L1 and L2 regularization (shrinkage and selection).

### Mathematical Formulation
Elastic Net modifies the cost function by combining the L1 and L2 penalties:

```
β̂ = argmin( ||Y - X * β||^2 + λ1 * ||β||_1 + λ2 * ||β||_2^2 )
```

Where:
- `λ1` controls the strength of the L1 penalty (Lasso part).
- `λ2` controls the strength of the L2 penalty (Ridge part).

Elastic Net optimizes both penalties simultaneously. A common implementation uses **cross-validation** to select the optimal values for both `λ1` and `λ2`.

### Key Differences from Lasso and Ridge:
- **Elastic Net** allows for a **combination of L1 and L2 penalties**, making it more flexible.
- Unlike Lasso, it can handle situations where there are highly correlated features by using the Ridge-like L2 penalty to stabilize the selection process.
- Unlike Ridge, it performs variable selection like Lasso.

### Pros:
- Works well when you have a large number of correlated predictors.
- Combines the advantages of both Lasso and Ridge (shrinkage and variable selection).
- More stable than Lasso when predictors are highly correlated.

### Cons:
- Requires tuning of two hyperparameters, `λ1` and `λ2`, which can increase computational cost.
- Might be overkill if Lasso or Ridge works well on its own.

---

## Summary of Key Differences

| **Method**          | **Penalty**         | **Variable Selection** | **Handling of Multicollinearity** | **Use Case**                                 |
|---------------------|---------------------|------------------------|-----------------------------------|----------------------------------------------|
| **Linear Regression**| None                | No                     | Poor                              | Small dataset, no multicollinearity, simple models |
| **Ridge Regression** | L2 (Squared)        | No                     | Good                              | When predictors are correlated, no need for feature selection |
| **Lasso Regression** | L1 (Absolute)       | Yes                    | Poor                              | Sparse models, when feature selection is needed |
| **Elastic Net**      | L1 + L2             | Yes                    | Good                              | When predictors are correlated and feature selection is needed |

---

## Choosing the Right Method

- **Linear Regression** is appropriate when you have few predictors and no issues with multicollinearity or overfitting.
- **Ridge Regression** is useful when predictors are correlated, and you want to shrink coefficients without removing any predictors.
- **Lasso Regression** is ideal when you suspect that only a few predictors are important and want to automatically perform feature selection.
- **Elastic Net** is the best option when you have many correlated predictors and want to leverage the benefits of both Lasso and Ridge regularization.

---

## Conclusion

Understanding the differences between these methods is crucial for selecting the right one for your regression tasks. While **Linear Regression** is a good starting point, the regularization techniques of **Ridge**, **Lasso**, and **Elastic Net** can significantly improve model performance, especially when dealing with high-dimensional datasets or multicollinearity.

For practical implementation, tuning of the regularization parameters (`λ1`, `λ2`) through **cross-validation** is often essential to ensure the best model performance.
