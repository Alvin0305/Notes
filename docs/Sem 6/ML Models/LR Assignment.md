# Question 2: Multiple Linear Regression

---
## 2.1 Target Function

In this problem, we aim to predict a student’s **Exam Score** using two independent variables: **Study Hours** and **Sleep Hours**.

### Mathematical Model
$y = w_1 \cdot x_1 + w_2 \cdot x_2 + b$
### Variable Definitions
- **y**: Exam Score (dependent variable)
- **$x_1$**: Study Hours (independent variable)
- **$x_2$**: Sleep Hours (independent variable)
- **$w_1$**: Weight associated with Study Hours
- **$w_2$**: Weight associated with Sleep Hours
- **$b$**: Bias (intercept) term

---

## 2.2 Model Results: _Without Feature Scaling_

### Code Implementation

```python
import numpy as np
from sklearn.linear_model import LinearRegression

# Training data
X = np.array([[1, 6], [2, 7], [3, 6], [4, 8]])
y = np.array([65, 70, 75, 85]) 

# Model Training
model1 = LinearRegression()
model1.fit(X, y)

w1, w2 = model1.coef_
b = model1.intercept_

# Predicting a score for (3 hours study, 7 hours sleep)
prediction = model1.predict([[3, 7]])[0]
```

### Results

| Parameter                 | Value    |
| ------------------------- | -------- |
| **$w_1$ (Study Hours)**   | 5.667    |
| **$w_2$ (Sleep Hours)**   | 1.667    |
| **$b$ (Intercept)**       | 48.33    |
| **Prediction for (3, 7)** | **77.0** |

**Interpretation:**

- Each additional hour of study increases the predicted exam score by **5.667 marks**, assuming sleep remains constant.
- Each additional hour of sleep contributes **1.667 marks** to the score.
- The intercept represents the baseline score when both study and sleep hours are zero.

---
## 2.3 Model Results: _With Feature Scaling_

### Code Implementation

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

model2 = LinearRegression()
model2.fit(X_scaled, y)

w1, w2 = model2.coef_
b = model2.intercept_

# Scale input for prediction
input_scaled = scaler.transform([[3, 7]])
prediction_scaled = model2.predict(input_scaled)[0]
```

### Results

| Parameter                        | Value    |
| -------------------------------- | -------- |
| **$w_1$ (Study Hours – Scaled)** | 6.335    |
| **$w_2$ (Sleep Hours – Scaled)** | 1.389    |
| **$b$ (Intercept)**              | 73.75    |
| **Prediction for (3, 7)**        | **77.0** |

### Key Observations

1. **Identical Predictions**  
    Both the scaled and unscaled models predict an exam score of **77.0** for the input (3, 7). Feature scaling does **not** change the final prediction for linear regression.    
2. **Different Coefficient Values**  
    The numerical values of the coefficients change because feature scaling standardizes the input features to a common range.
3. **Why Feature Scaling Matters**
    - Improves numerical stability during optimization
    - Speeds up convergence in gradient-based algorithms
    - Makes coefficients easier to compare
    - Prevents features with larger magnitudes from dominating the model
---

## 2.4 Convergence Analysis (Effect of Learning Rate)

To study the impact of the **learning rate**, we trained a model using `SGDRegressor` for 100 iterations and observed the cost reduction.

| Learning Rate | Behavior                   | Type               | Explanation                                                                      |
| ------------- | -------------------------- | ------------------ | -------------------------------------------------------------------------------- |
| **0.01**      | Smooth, gradual decrease   | Slow Convergence   | The step size is very small, resulting in stable but slow learning.              |
| **0.1**       | Fast and smooth decrease   | Stable Convergence | **Optimal learning rate**, providing a good balance between speed and stability. |
| **1.0**       | Oscillations or divergence | Unstable           | The step size is too large, causing the algorithm to overshoot the minimum.      |
![[Pasted image 20260121201429.png]]
---

# Question 3: Polynomial Regression

## 3.1 Data Overview and Pattern Analysis

We analyze the relationship between input variable x and output variable y using the following dataset:

| x   | y   |
| --- | --- |
| 1   | 1   |
| 2   | 5   |
| 3   | 10  |
| 4   | 18  |
| 5   | 30  |

![[Pasted image 20260121202435.png]]

### Observed Pattern

From the scatter plot, its visible that the data is not completely linear but has a small polynomial behaviour

---

## 3.2 Model Comparison

Three different models were trained and compared: Linear, Polynomial (Degree 2), and Polynomial (Degree 4).

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression

x = np.array([1, 2, 3, 4, 5]).reshape(-1, 1)
y = np.array([1, 5, 10, 18, 30])

def train_linear(x, y):
    model = LinearRegression()
    model.fit(x, y)
    return model, model.predict(x)

def train_polynomial(x, y, degree):
    features = PolynomialFeatures(degree=degree)
    x_poly = features.fit_transform(x)
    model = LinearRegression()
    model.fit(x_poly, y)
    return model, features, model.predict(x_poly)

def compute_cost(y_true, y_pred):
    return np.mean((y_pred - y_true) ** 2) / 2

plt.figure(figsize=(6, 5))
plt.scatter(x, y, color='navy', s=120, edgecolors='black', label='Actual Data Points')
plt.xlabel('x')
plt.ylabel('y')
plt.title('Scatter Plot: x vs y')
plt.grid(True, linestyle='--', alpha=0.6)
plt.legend()
plt.show()

linear_model, y_pred_linear = train_linear(x, y)
print(f"\nLinear function: y = {linear_model.coef_[0]:.2f}*x + {linear_model.intercept_:.2f}\n")

poly2_model, poly2_features, y_pred_poly2 = train_polynomial(x, y, 2)
a0 = poly2_model.intercept_
a1, a2 = poly2_model.coef_[1:3]
print("Polynomial function (degree = 2):")
print(f"y = {a2:.3f}x^2 + {a1:.3f}x + {a0:.3f}\n")

poly4_model, poly4_features, y_pred_poly4 = train_polynomial(x, y, 4)
a0 = poly4_model.intercept_
a1, a2, a3, a4 = poly4_model.coef_[1:5]
print("Polynomial function (degree = 4):")
print(f"y = {a4:.3f}x^4 + {a3:.3f}x^3 + {a2:.3f}x^2 + {a1:.3f}x + {a0:.3f}")

cost_linear = compute_cost(y, y_pred_linear)
cost_poly2 = compute_cost(y, y_pred_poly2)
cost_poly4 = compute_cost(y, y_pred_poly4)

print(f"\nLinear Model Cost: {cost_linear:.4f}")
print(f"Polynomial (Degree 2) Model Cost: {cost_poly2:.4f}")
print(f"Polynomial (Degree 4) Model Cost: {cost_poly4:.4f}\n")

x_line = np.linspace(1, 5, 100).reshape(-1, 1)

plt.figure(figsize=(14, 6))

plt.subplot(1, 2, 1)
plt.scatter(x, y, color='navy', s=120, edgecolors='black', label='Actual Data')
plt.plot(x_line, linear_model.predict(x_line), 'r--', linewidth=2, label='Linear Model')
plt.plot(x_line, poly2_model.predict(poly2_features.transform(x_line)), 'g-', linewidth=2, label='Polynomial (Degree 2)')
plt.plot(x_line, poly4_model.predict(poly4_features.transform(x_line)), 'p:', linewidth=3, label='Polynomial (Degree 4)')
plt.xlabel('x')
plt.ylabel('y')
plt.title('Linear vs Polynomial Regression')
plt.legend()
plt.grid(True, linestyle='--', alpha=0.6)

plt.subplot(1, 2, 2)
models = ['Linear', 'Polynomial (Deg 2)', 'Polynomial (Deg 4)']
costs = [cost_linear, cost_poly2, cost_poly4]
bars = plt.bar(models, costs, color=['green', 'purple', 'blue'])
plt.ylabel('Cost (MSE / 2)')
plt.title('Model Cost Comparison')
plt.grid(True, axis='y', linestyle='--', alpha=0.6)

for bar in bars:
    plt.text(bar.get_x() + bar.get_width() / 2, bar.get_height(), f'{bar.get_height():.3f}', ha='center', va='bottom')

plt.tight_layout()
plt.show()

test_points = np.array([[2.5], [3.5], [4.5]])

pred_linear = linear_model.predict(test_points)
pred_poly2 = poly2_model.predict(poly2_features.transform(test_points))
pred_poly4 = poly4_model.predict(poly4_features.transform(test_points))

print("Predictions:")
for i, val in enumerate(test_points):
    print(f"x = {val[0]}: Linear = {pred_linear[i]:.2f}, Polynomial(2) = {pred_poly2[i]:.2f}, Polynomial(4) = {pred_poly4[i]:.2f}")
```
### Learned Model Equations

- **Linear Regression**  
    $\hat{y} = 7.10x - 8.5$    
- **Polynomial Regression (Degree 2)**  
    $\hat{y} = 1.357x^2 - 1.043x + 1.000$
- **Polynomial Regression (Degree 4)**  
    $\hat{y} = -0.042x^4 + 0.750x^3 - 2.958x^2 + 8.250x - 5.000$    

### Performance Comparison (Cost = MSE/2)
![[Pasted image 20260121202109.png]]

| Model        | Cost (MSE/2) | Interpretation                                    |
| ------------ | ------------ | ------------------------------------------------- |
| **Linear**   | 2.67         | Underfits the data and fails to capture curvature |
| **Degree 2** | 0.0914       | Best fit with smooth curvature                    |
| **Degree 4** | 0.0          | Perfect fit but overly complex (overfitting)      |

---
## 3.3 Predictions on Unseen Data

To evaluate generalization, predictions were made for unseen values of x.

| x       | Linear | Polynomial (Deg 2) | Polynomial (Deg 4) |
| ------- | ------ | ------------------ | ------------------ |
| **2.5** | 9.25   | 6.88               | 6.88               |
| **3.5** | 16.35  | 13.98              | 13.98              |
| **4.5** | 23.45  | 23.79              | 23.79              |

---
## 3.4 Model overfitting, underfitting

Based on the analysis:

- **Best Model: Polynomial Regression (Degree 2)**
    - Captures the quadratic relationship effectively
    - Achieves very low training error
    - Maintains a good balance between **bias and variance**
    - Avoids unnecessary complexity
- **Linear Regression** underfits due to limited model capacity.
- **Higher-degree polynomials (Degree ≥ 3)** tend to overfit, especially given the small dataset size.