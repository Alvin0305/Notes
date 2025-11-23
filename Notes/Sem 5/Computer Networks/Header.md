#### Ethernet Header
- Preamble -> 7B
- Delimiter -> 1B
- Destination MAC -> 6B
- Source MAC -> 6B
- Length -> 2B
- Payload -> (not part of the header, this is the data)
- CRC -> 4B (not part of the header, this is the trailer)
####  IPv4 Header (20 to 60 Bytes)
- Row I
	- Version -> 4 bits
	- Header Length -> 4 bits
	- Differentiating service -> 16 bits
	- Total Size -> 8 bits
- Row II
	- Fragmentation ID -> 12 bits
	- Reserved -> 1 bit
	- DF -> 1 bit
	- MF -> 1 bit
	- Fragmentation Offset -> 13 bits
- Row III
	- TTL -> 8 bits
	- Protocol -> 8 bits
	- Check Sum -> 16 bits
- Row IV
	- Sender IP -> 32 bits
- Row V
	- Receivers IP -> 32 bits
- Row VI
	- Options -> 40 Bytes
	- Padding -> variable

#### TCP Header (20 to 60 Bytes)
- Row I
	- Sender Port No. -> 16 bits
	- Receiver Port No. -> 16 bits
- Row II
	- Sequence No. -> 32 bits
- Row III
	- Acknowledgement No. -> 32 bits
- Row IV
	- Header Length -> 4 bits
	- Reserved -> 6 bits
	- Flags -> 6 bits
		- URGENT
		- ACK
		- PUSH
		- RESET
		- SYN
		- FIN
	- Window Size -> 16 bits
- Row V
	- Check sum -> 16 bits
	- Urgent Pointer -> 16 bits
- Row VI
	- Options -> variable
	- Padding -> variable

#### ARP Header (28 Bytes)
- Row 1
	- Hardware Type -> 16 bits
	- Protocol Type -> 16 bits
- Row 2
	- Hardware Size -> 8 bits (length of hardware address = 6 for MAC)
	- Protocol Size -> 8 bits (length of protocol address = 4 for IPv4)
	- Opcode -> 16 bits (1 for Request, 2 for Response)
- Sender MAC Address (48 bits)
- Sender IP Address (32 bits)
- Receiver MAC Address (48 bits)
- Receiver IP Address (32 bits)





































