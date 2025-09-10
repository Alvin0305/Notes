### Fermat's Little Theorem
---
- If p is a prime number, then for all integer a < p, 
$$
a^{p-1} = 1 \text{ mod p}
$$

### Masters Theorem
---
$$
\begin{aligned}
\text{For } T(n) &= aT(n/b) + O(n^k) \\
\text{Find } x &= log_b{a} \\
\text{If } x \lt k: T(n) &= O(n^k) \\
\text{If } x \gt k: T(n) &= O(n^x) \\
\text{If } x = k: T(n) &= O(n^xlog^{k + 1}n) \\
\end{aligned}
$$
### Extended Masters Theorem
---
$$
\begin{aligned}
\text{General Form: }\\
T(x) &= \sum\limits_{i=1}^k a_iT(b_ix) + g(x) \\
\text{where } 0 < b_i &< 1 \\
\text{and } a_i &> 0 \\
\text{and } g(x) &\text{is the cost outside the recusion(like splitting, merging)}
\end{aligned}
$$
- Define p such that:
$$
\sum\limits_{i=1}^k a_ib_i^p = 1
$$
- Compare g(x) with $x^p$
- If g(x) grows faster than $x^p$, then T(x) = O(g(x))
- If $x^p$ grows faster than g(x), then T(x) = $x^p$
- If both are asymptotically same, then T(x) = O($x^plogx$)

>[!example]
>Quick Sort
>- T(n) = T(n/4) + T(3n/4) + O(n)
>- Here, $a_1 = 1, b_1 = 1/4, a_2 = 1, b_2 = 3/4$
>- Solve for p
>	- => $(1)(\frac{1}{4})^p + (1)(\frac{3}{4})^p = 1$
>	- => p = 1
>- Now we compare $n^p$ and O(n)
>- i.e., we compare O(n) and O(n)
>- both are asymptotically same => We take $T(n) = O(n^plogn)$
>- => T(n) = O(nlogn)

>[!example]
>Median of Medians Algorithm
>- T(n) <= T(7n/10) + T(n/5) + O(n)
>- Here $a_1 = 1, b_1 = 7/10, a_2 = 1, b_2 = 1/5$
>- Solve for p:
>	- => $(1)(\frac{7}{10})^p + (1)(\frac{1}{5})^p = 1$
>	- => $p \approx 0.85$
>- Now we compare $n^p$ and O(n)
>- i.e., we compare $O(n^{0.85})$ with O(n)
>- O(n) grows faster, so we takes O(n)
>- => T(n) = O(n)
