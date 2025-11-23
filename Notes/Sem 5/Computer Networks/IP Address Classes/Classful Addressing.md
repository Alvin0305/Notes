### Classes
- Class A
- Class B
- Class C
- Class D
- Class E

>[!info] 
>Class A, B and C are normally found in internet
#### Class A
- In binary notation, starts with 0
- Range -> 0.0.0.0 to 127.255.255.255
- Default Mask -> 255.0.0.0
- \#networks = $2^7 = 128$ 
- \#hosts = $2^{24} - 2 = 16777214$
#### Class B
- In binary notation, starts with 10
- Range -> 128.0.0.0 to 191.255.255.255
- Default Mask -> 255.255.0.0
- \#networks = $2^{6 + 8} = 2^{14} = 16384$
- \#hosts = $2^{16} - 2 = 65534$
#### Class C
- In binary notation, starts with 110
- Range -> 192.0.0.0 to 223.255.255.255
- Default Mask -> 255.255.255.0
- \#networks = $2^{5 + 8 + 8} = 2^{21} = 209150$
- \#host = $2^8 - 2 = 254$
#### Class D
- In binary notation, starts with 1110
- Range -> 224.0.0.0 to 239.255.255.255
- Default Mask -> NA
- Multicast (IGMP)
#### Class E
- In binary notation, starts with 1111
- Range -> 240.0.0.0 to 255.255.255.255
- Default Mask -> NA
- Military use

![[Classful Addressing.png]]

| IP           | Class |
| ------------ | ----- |
| 192.168.1.10 | C     |
| 10.10.200.6  | A     |
| 172.15.165.1 | B     |
| 230.10.65.30 | D     |
