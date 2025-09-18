#### [[OSI Model]]
- Open Systems Interconnection Model
- Developed by ISO in 1980
- Became standard at 1984
- Properly designed
- Academic and theoretical only
- 7 layers
	- Application
	- Presentation
	- Session
	- Transport
	- Network
	- Data link
	- Physical

#### TCP/IP Model
- Developed by DARPA for ARPANET in 1969
- Became standard by 1983
- Not properly designed
- But used throughout internet
- 4 layers
	- Application => All high level protocols (HTTP, FTP, SMTP, DNS) 
		- Merges Application, Presentation and Session layer in OSI Model
	- Transport => End-to-end delivery (TCP, UDP)
		- same as Transport layer in OSI Model
	- Internet => Logical addressing & routing (IP, ARP)
		- same as Network layer in OSI Model
	- Network Access (Link) => Ethernet, WiFi
		- Merges Physical and Data link layer in OSI model
- Error and flow control in Data link layer is moved to Transport layer
- Application layer is in User space
- Transport, Internet and Network Access (Link) layer is in Kernel space
- No layer is in hardware

In OSI model since routers and switches have Data link layer, error and flow control is done through out the path, but in TCP/IP, its done only in the destination device

Header which we pass includes, [[Network Identifiers#PORT-Number|Port No]] (16 bit), [[Network Identifiers#IP-Address|IP Address]] (32 bit), [[Network Identifiers#MAC-Address|MAC Address]] (48 bit) of both the sender and receiver
Therefore, there will be a overhead of (16 + 32 + 48) * 2 = 192 bits