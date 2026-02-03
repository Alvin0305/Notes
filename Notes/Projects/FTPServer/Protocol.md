### Request
```C++
Method PayloadSize\n
key : value\n
key : value\n
...\n
\n
data
...
```
### Response
```C
Method PayloadSize StatusCode\n
key : value\n
key : value\n
...\n
\n
data
...
```
#### GET
- Request
```C++
GET 0\n
filepath : file.txt\n
\n
```
- Response
```C++
200 GET 10\n
\n
\n
helloooooo
```
