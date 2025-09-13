- Generate code from output of analysis
- This is considered the back end part

### Code Improving Transformations
---
- Code improving transformations are machine independent / dependent

>[!example]
>- i = 5
>- t1 = i \* 10
>- t2 = sum + t1
>- x = t2
>  
>  After code improving transformation
>- t1 = 5 \* 10
>- t2 = sum + t1
>- x = t2
>  
>  removed one load instruction


