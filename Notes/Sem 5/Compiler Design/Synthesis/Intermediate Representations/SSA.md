#### Static Single Assignment (SSA) Form
- All assignments are to variables with distinct names.
- SSA facilitates certain code optimizations.
- Definitions of the same variable are changed to definitions of distinct variables by renaming of variables:

>[!example] 
>- x = a + b
>- x = p + q
>is converted to 
>- x1 = a + b
>- x2 = p + q

>[!info] In branches
>- if (flag) x = 1 else x = 2
>- y = x + 1
>this can be converted as
>- if (flag) x1 = 1 else x2 = 2
>- y = $\phi(x_1, x_2) + 1$  

- $\phi$ is a theoretical concept. Not used in implementation
