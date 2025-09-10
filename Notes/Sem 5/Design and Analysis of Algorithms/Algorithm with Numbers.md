- Here, Complexity refers to bit complexity. i.e., number of operations we have to do on a bit (not a number as a whole)

| Operation              | Complexity |
| ---------------------- | ---------- |
| Addition               | O(n)       |
| Subtraction            | O(n)       |
| Multiplication         | O($n^2$)   |
| Division               | O($n^2$)   |
| Modular Exponentiation | O($n^3$)   |
| GCD (Euclid)           | O($n^3$)   |
| Modular Division       | O($n^3$)   |
#### Addition
---
- Result of two n bits numbers result in a sum of at most n + 1 bits
- Complexity is O(n)
#### Subtraction
---
- Same as Addition
#### Multiplication
---
```Python
def multiply(x, y):
	if y == 0: # O(n) for comparing each bit with 0
		return 0 # O(1)
	z = multiply(x, floor(y / 2)) # O(1)
	if y is even: # O(1) only the last bit has to be checked
		return 2z # O(n) -> n bit shift
	else: 
		return x + 2z # O(n) -> n bit shift + n bit addition
```

- Shift operation requires O(n) time
- Odd or even check takes only O(1) time (only the last bit has to be checked)
- Number of recursive calls:
- `log(y) = log(2^n) = n`
- Cost of each recursive calls: O(n) time (maximum of O(1) and O(n))
- So total cost = O($n^2$)

>[!example] 
>18 * 15

| Recursive Calls | Return Values    |
| --------------- | ---------------- |
| 18, 15          | 126*2 + 18 = 270 |
| 18, 7           | 54*2 + 18 = 126  |
| 18, 3           | 18*2 + 18 = 54   |
| 18, 1           | 0*2 + 18 = 18    |
| 18, 0           | 0                |
#### Division
---
```Python
def divide(x, y):
	if x == 0: # O(n) for comparing each bit with 0
		return (0, 0) # O(1)
	q, r = divide(floor(x / 2), y) # O(1)
	q = 2q # O(n) shift
	r = 2r # O(n) shift
	if x is odd: # O(1) last bit check
		r = r + 1 # O(n) addition
	if r >= y: # O(n) comparison
		r = r - y # O(n) subtraction
		q = q + 1 # O(n) addition
	return (q, r)
```

- Number of recursive calls:
- `log(x) = log(2^n) = n`
- Each recursive calls takes O(n) time (maximum of O(1) and O(n))
- Therefore, total time complexity is O($n^2$)

>[!example]
>113 / 5

| Recursive Calls | Return Values                      |
| --------------- | ---------------------------------- |
| 113, 5          | (11\*2, 1\*2) = (22, 2)            |
| 56, 5           | (2\*5, 2\*3) = (10, 6) = (11, 1)   |
| 28, 5           | (2\*2, 4\*2) = (4, 8) = (5, 3)     |
| 14, 5           | (1\*2, 2\*2) = (2, 4)              |
| 7, 5            | (0\*2, 2\*3 + 1) = (0, 7) = (1, 2) |
| 3, 5            | (0\*2, 2\*1 + 1) = (0, 3)          |
| 1, 5            | (0\*2, 2\*0 + 1) = (0, 1)          |
| 0, 5            | (0, 0)                             |
#### Modular Exponentiation
---
- Given X, Y and N, we need to find $X^Y (mod N)$

```Python
def modexp(x, y, N):
	if y == 0: # O(n) each bit compared to 0
		return 1 # O(1)
	z = modexp(x, floor(y / 2), N) # O(1)
	if y is even: # O(1) last bit compared
		return z^2 mod N # O(n^2) for squaring and O(n^2) for taking modulus by division
	else 
		return x * z^2 mod N # O(n^2) for squaring, O(n^2) for multiplication and O(n^2) for taking modulus by division
```

>[!example] 
>$20^{17} mod 15$

| Recursive Calls | Return Values          |
| --------------- | ---------------------- |
| 20, 17, 15      | $20 * 10^2 mod 15$ = 5 |
| 20, 8, 15       | $10^2 mod 15$ = 10     |
| 20, 4, 15       | $10^2 mod 15$ = 10     |
| 20, 2, 15       | $5^2 mod 15$ = 10      |
| 20, 1, 15       | $20 * 1^2 mod 15$ = 5  |
| 20, 0, 15       | 1                      |
- Total number of recursive calls:
- `log(y) = log(2^n) = n`
- Each recursive call takes $O(n^2)$ time
- Therefore, total time = $O(n^3)$

#### Euclid's Algorithm (Finding GCD)
---
- We need to find the GCD of two numbers
- According to Algorithm (considering a >= b)
- `gcd(a, b) = gcd(b, a mod b)`

```Python
def gcd(a, b):
	if b == 0: # O(n) n bit comparison with 0 
		return a # O(1)
	return gcd(b, a mod b) # O(n^2) modulus requires O(n^2) division
```

- Each recursive call required $O(n^2)$ time

> [!quote] Lemma
> If a >= b, then a mod b < a / 2
> 
> Proof: 
> Either b <= a / 2 or b > a / 2. 
> If b <= a / 2, a mod b will always be less than b => a mod b < b. And we have b <= a / 2 => a mod b < b <= a / 2 => a mod b < a / 2
> If b > a / 2, then we can have only one b inside a and the remaining will be obviously less than a / 2 since b has already taken a / 2.

From the above lemma, we can see that, at each step the number will be reduced by at least half. i.e., the length of the number reduces by at least 1 bit in every recursive call. So, after maximum 2n recursive calls, we will reach the base case. So at maximum there will be O(n) recursive calls. Therefore the total complexity is $O(n^3)$

>[!quote] Proof
> We need to prove gcd(a, b) = gcd(b, a mod b)
> i.e., gcd(a, b) = gcd(b, r) where r = a mod b
> - Suppose d|a and d|b
> - a = bq + r
> - => r = a - bq
> - Since d divides both a and b, it divides a and bq => it divides r
>
>So, any common divisor of (a, b) also divides (b, r)
> - Now, d|b and d|r
> - a = bq + r 
> - Since d|b and d|r, d divides bq and d divides r => d divides a
> 
> So, any common divisor of (b, r) also divides (a, b)
> i.e., common divisors of (a, b) = common divisors (b, r)
> So their gcd is also the same
> => gcd(a, b) = gcd(b, r)

>[!info]
>Worst case for Euclid's Algorithm happens when the numbers are consecutive Fibonacci numbers
#### Extended Euclid's Algorithm
---
>[!quote] Lemma
>if d divides a and b and d = ax + by for some integer x and y then d = gcd(a, b)
>
>Proof: 
>- Since d divides a and b, it is a common divisor. So it cannot exceed the greatest common divisor of a and b. i.e, d <= gcd(a, b)
>- Since, gcd divides a and b, it divides any linear combination of a and b, i.e., gcd divides ax + by. 
>- => gcd <= ax + by
>- => gcd <= d
>- => gcd <= d and gcd >= d => gcd = d

- Extended Euclid's Algorithm gives us both the gcd (d) as well as x and y such that d = gcd(a, b) = ax + by

```Python
def extended_euclid_algo(a, b):
	if b == 0:
		return (a, 1, 0)
	else:
		(d, x, y) = extended_euclid_algo(b, a mod b)
		return (d, y, x - floor(a / b) * y)
```

>[!example] 
>(21, 18)

| Recursive Calls | Return Value                                |
| --------------- | ------------------------------------------- |
| 21, 18          | (3, 1, 0 - floor(21 / 18) * 1) = (3, 1, -1) |
| 18, 3           | (3, 0, 1 - floor(21 / 18) \* 0) = (3, 0, 1) |
| 3, 0            | (3, 1, 0)                                   |

>[!quote] Correctness of proof
>We know, $a = \left \lfloor{\frac{a}{b}}\right \rfloor b + a mod b$
>And, $d = gcd(b, a mod b) \implies d = x'b + y'amodb$
>=> $d = x'b + y'\{a - \left \lfloor{\frac{a}{b}}\right \rfloor b\}$
#### Modular Division
---
- We say x is the multiplicative inverse of a modulo N if ax ≡ 1 mod N

>[!important]
>For any a mod N , a has a multiplicative inverse modulo N if and only if it is relatively prime to N . When this inverse exists, it can be found in time $O(n^3)$ by running the extended Euclid algorithm.

- i.e., when we run an extended-euclid algorithm, we find x and y such that, ax + by = gcd(a, N)
- but here, gcd(a, N) = 1
- => ax + Ny = 1
- => ax mod N + Ny mod N = 1 mod N
- => ax mod N + 0 = 1 mod N
- => ax mod N = 1 mod N
- => ax ≡ 1 mod N
- => $a^{-1} = x$ 

#### Fibonacci Numbers
- Finding Fibonacci numbers using DP takes O(n) time.
- But if we use matrices, we can find it in O(logN) number of matrix multiplications

$$
\begin{pmatrix}
F_{2}\\
F_{1}\\
\end{pmatrix}
= 
\begin{pmatrix}
1 & 1\\
1 & 0\\
\end{pmatrix}
\begin{pmatrix}
F_{1}\\
F_{0}\\
\end{pmatrix}
$$
- Using this matrix, we can find F2 and F1
- By doing this again and again
$$
\begin{pmatrix}
F_{k+1}\\
F_{k}\\
\end{pmatrix}
= 
\begin{pmatrix}
1 & 1\\
1 & 0\\
\end{pmatrix}^{k}
\begin{pmatrix}
F_{1}\\
F_{0}\\
\end{pmatrix}
$$
- This does not required k matrix multiplications
- This requires only log k number of matrix multiplications
- Therefore by this method, we can efficiently find Fibonacci numbers
$$
\begin{array}{l}
z = \begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix} \\[6pt]
\text{if $k$ is even:} \\
\quad \text{return } z^2 \\[6pt]
\text{else:} \\
\quad \text{return } \begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix} \cdot z^2
\end{array}
$$

### Prime Number Theorem
---
$\lim_{n\rightarrow \infty} {\frac{\pi_n}{\frac{n}{lnn}} = 1}$
$\text{for very large n: } \pi_n \approx \frac{n}{lnn}$ 
- roughly every ln(n)-th number near n is prime
### Primality Test
---
- First approach is to divide n by 1 to $\sqrt{n}$. If none of the remainders are 0, then n is prime. (we can avoid even integers > 2) 
- But, it will take $O(\sqrt{n})$ time = $O(n^{1/2}) = O(2^N/2) = O(2^{N - 1})$ which is exponential
- But this method gives us unnecessary information(We just need to check if the number is prime or not, but this method tells us the factors of the number, which is unnecessary)
- The best method to use is Pseudo Primality Testing
### Pseudo Primality Testing
---

- This is a method for primality testing that "almost works". In fact, is good enough for many practical applications
- The idea is:
	- if n is prime, then [[Notations#$Z_n +$|Zn+]] = [[Notations#$Z_n *$|Zn*]]
	- Because, if n is prime, then all the numbers between 1 and n will be co-prime to n.
- We use [[Useful Theorems#Fermat's Little Theorem|Fermat's Little Theorem]] here
- So we take a number "a" which is less than n
- Find $a^{p-1} \text{ mod p}$. If it is 1, then the n is prime, otherwise n is composite
- Modular exponentiation takes only $O(n^3)$ time. Therefore the complexity of this method is $O(n^3)$
- But this method can give false positives
	- i.e., If a number is prime, then the algorithm would always say it is prime
	- But is the number is composite, then it is not guaranteed that it will say it is composite
	- In such cases, we need to take multiple values of a to check (If the number is prime, then for all a, the result will be 1)
	- But in that case also, there are specific numbers called Carmicheal's numbers which gives wrong result(is composite but will give 1 for all values of 1)

>[!tip]
>Carmicheal Numbers examples: 561
