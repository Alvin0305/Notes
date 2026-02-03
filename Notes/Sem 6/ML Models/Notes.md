#### Gradient Descent
$$
w = w - \alpha\frac{\partial J(w, b)}{\partial w}
$$
$$
b = b - \alpha\frac{\partial J(w, b)}{\partial b}
$$
- $\alpha$ is the learning rate
$$f(x) = wx + b$$
- when the number of features is >= 2, wx becomes dot product

### Regression Metrics

- MSE
$$
J(w, b) = \frac{1}{n}\Sigma{(f(x^{(i)}) - y^{(i)})^2}
$$
- MAE
$$
J(w, b) = \frac{1}{n}\Sigma{|f(x^{(i)}) - y^{(i)}|}
$$
- Root Mean Squared Error
$$
J(w, b) = \sqrt{\frac{1}{n}\Sigma{(f(x^{(i)}) - y^{(i)})^2}}
$$
- R-squared Score
$$
R_2 = 1 - \frac{SSR}{SST}
$$
