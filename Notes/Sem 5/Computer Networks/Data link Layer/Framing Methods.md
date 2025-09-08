The data data link layer receives from physical layer is just a stream of bits which doesn't have any meaning until it is framed, so that we can separate the header, body and trailer. This is done in data link layer and this process is called framing. This can be done in 3 ways
- Byte count
- Byte stuffing
- Bit stuffing

### Byte Count
---
the size of the frame is kept in the starting of the frame.

>[!example]
>```
>Data = HELLO, WORLD!!
>[5 H E L L O] [7 W O R L D ! !]
>```

>[!warning]
>If the count byte itself gets corrupted, then the entire frame gets corrupted

### Byte Stuffing
---
We use delimiters at starting and ending of the frame. Each delimiter is of size 8 bits (1 Byte). If the delimiter itself appear in the body, we escape it by escape character

- DLE STX => Start of Frame
- DLE ETX => End of Frame
- DLE => Escape character

>[!example]
>```
>Data = A B DLE C D
>Frame = DLE STX A B DLE DLE C D DLE ETX
>```
>Receiver sees DLE STX and understands, it is the starting of the frame
>two DLE means => interprets it as a single one
>DLE ETX => it is the end of the frame

#### Disadvantage
- Extra overhead due to escape characters
- Can be used only for byte-based encodings, not efficient for arbitrary bit streams

### Bit Stuffing
---
special flag sequence (e.g,. 01111110) is used to mark starting and ending of frames. If the same pattern appears in the data itself, the sender inserts a 0 after every sequence of five consecutive 1s. Receiver removes the stuffed bits.

>[!example]
>```
>Flag = 01111110
>Data = 01111101111110
>Stuffed Data = 0111110 0 111110 10
>Frame = Flag + Stuffed Data + Flag
>	 = 01111110 0111110011111010 01111110
>```
>
>Receiver gets the frame, finds, Flag in the start, removes the flag
>Scans the body, when it receives 5 consecutive 1s, it removes the next 0
