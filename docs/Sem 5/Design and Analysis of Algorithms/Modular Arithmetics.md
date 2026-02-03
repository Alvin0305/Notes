- (x ≡ y) mod N means N divides (x - y)
- Another interpretation is that modular arithmetic deals with all the integers, but divides them into N equivalence classes, each of the form {i + kN : k ∈ Z} for some i between 0 and N − 1. For example, there are three equivalence classes modulo 3

>[!example]
>· · · −9 −6 −3 0 3 6 9 · · ·
· · · −8 −5 −2 1 4 7 10 · · ·
· · · −7 −4 −1 2 5 8 11 · · ·

- Any member of an equivalence class is substitutable for any other; when viewed modulo 3, the numbers 5 and 11 are no different. Under such substitutions, addition and multiplication remain well-defined.

>[!important] Substitution Rule
> if x ≡ x' (mod N) and y ≡ y' (mod N), then
> ```
> x + y ≡ x' + y' (mod N) and
> xy ≡ x'y' (mod N)
> ```

>[!example]
>$2^{345} \equiv (2^5)^{69} \equiv 32^{69} \equiv 1^{69} \equiv 1 (mod 31)$

>[!important]
>Associativity `x + (y + z) ≡ (x + y) + z (mod N)` 
>Commutativity `xy ≡ yx (mod N)` 
>Distributivity `x(y + z) ≡ xy + xz (mod N)` 

