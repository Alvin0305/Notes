##### What to find
$$
y_{pred} = mx + b
$$
##### Error
$$
\begin{aligned}
E&=\frac{1}{n}\sum_{i=0}^n(y_i - y_{pred})^2 \\
E&=\frac{1}{n}\sum_{i=0}^n(y_i - (mx_i + b))^2 \\
\end{aligned}
$$
##### Optimal values of m and b
- We find the derivative of **E** w.r.t **m** and **b** (increases E) and take its opposite (decrease E)
$$
\begin{aligned}
m = m - L\times \frac{\partial E}{\partial m} \\
b = b - L\times \frac{\partial E}{\partial b} \\
\end{aligned}
$$
- L is the learning rate => how fast we are stepping up. Larger => faster, less accurate, smaller => slow, more accurate

