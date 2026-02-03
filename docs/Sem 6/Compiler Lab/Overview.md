- 20 General Purpose Registers R0 -> R19
- 3 Special Purpose Registers BP, IP, SP
- Memory Address space is of size 5120 words

| Name    | Start Address | End Address |
| ------- | ------------- | ----------- |
| Library | 0             | 1023        |
| Heap    | 1024          | 2047        |
| Header  | 2048          | 2055        |
| Code    | 2056          | 4095        |
| Stack   | 4096          | 5120        |

| SysCall | Interrupt Routine Number |
| ------- | ------------------------ |
| Read    | 6                        |
| Write   | 7                        |
| Exit    | 10                       |

| Header       | Value                                                  |
| ------------ | ------------------------------------------------------ |
| Magic Number | 0                                                      |
| Entry Point  | 2056 (Starting of Code)                                |
| Text Size    | 0 (2048 is the correct one)                            |
| Data Size    | 0 (actual value should be calculated from global vars) |
| Heap Size    | 0 (1024 is the correct one)                            |
| Stack Size   | 0 (1024 - data size is the correct one)                |
| Library Flag | 0                                                      |
| Unused       | 0                                                      |

| Library Function | Function Code | Arg 1   | Arg 2         | Arg 3 |
| ---------------- | ------------- | ------- | ------------- | ----- |
| Read             | "Read"        | -1      | Memory Addr   |       |
| Write            | "Write"       | -2      | Data to write |       |
| Exit             | "Exit"        |         |               |       |
| Initialize       | "Heapset"     |         |               |       |
| Alloc            | "Alloc"       | size    |               |       |
| Free             | "Free"        | pointer |               |       |

| Read Return Value | Meaning                 |
| ----------------- | ----------------------- |
| 0                 | Success                 |
| -1                | Invalid File Descriptor |
| -2                | Read Error              |

| Write Return Value | Meaning                 |
| ------------------ | ----------------------- |
| 0                  | Success                 |
| -1                 | Invalid File Descriptor |

| Initialize Return Value | Meaning |
| ----------------------- | ------- |
| 0                       | Success |
| -1                      | Failure |

| Alloc Return Value | Meaning |
| ------------------ | ------- |
| Address in heap    | Success |
| -1                 | Failure |

| Free Return Value | Meaning |
| ----------------- | ------- |
| 0                 | Success |
| -1                | Failure |

| System Call | System Call Number | Interrupt Routine Number | Arg 1 | Arg 2  | Arg 3 |
| ----------- | ------------------ | ------------------------ | ----- | ------ | ----- |
| Read        | 7                  | 6                        | -1    | Buffer |       |
| Write       | 5                  | 7                        | -2    | Data   |       |
| Exit        | 10                 | 10                       |       |        |       |
