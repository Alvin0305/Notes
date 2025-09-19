- Application Layer
- Presentation Layer
- Session Layer
- Transport Layer
- Network Layer
- Data link Layer
- Physical Layer

#### Application Layer
- Provide network services directly to the user/application
- example: Web Browsers, email clients, file transfer apps
- Protocols: 
	- HTTP
	- HTTPS
	- SMTP
	- FTP
	- DNS

#### Presentation Layer
- Translates, Compresses or encrypts data so application can understand it
- Functions:
	- Data format conversions -> JPEG, MP4, PDF
	- Encryption, Decryption -> SSL, TLS
	- Compression -> ZIP, MPEG

#### Session Layer
- Manages session between two systems
- Functions:
	- Establishing, maintaining and ending sessions
	- Synchronization in data transfer
- Protocols:
	- NetBIOS
	- RPC

#### Transport Layer
- Ensure end-to-end communication with reliability and error-checking
- Functions:
	- Split data into Segments
	- Re-transmit if data is lost
	- Error detection
	- Can be connection oriented(TCP) or connection less(UDP)
	- Ordering the received packets
- Protocols
	- TCP
		- Before the communication starts, a three way hand shake is done to establish the connection 
			1. client sends a SYN to the server
			2. on receiving the SYN, the server sets the ACK flag and sends a SYN/ACK to the client
			3. the client receives the SYN/ACK, and sends a final ACK completing the three way hand-shake establishing a reliable connection
		- After the connection is established, the devices can send and receive data, if data is lost or acknowledgement is lost, the packet is resent
		- Slower due to overhead of connection establishment and error checking
		- Application: Email, File transfer, Web browsing
	- UDP
		- No need of a pre-established connection
		- No error checking or guaranteed delivery
		- Faster than TCP since, there is no error checking or connection establishment
		- Application: Video streaming, online gaming, broadcasting

#### Networks Layer
- Handles logical addressing and routing
- Functions:
	- Assign IP addresses
	- Finds the best path for data (routing)
	- Delivers packets to the correct device on a different network
- Protocols:
	- IP(IPv4, IPv6)
	- ARP
- Devices:
	- Router

#### [[Data link Layer]]
- Handles physical addressing (MAC) and error detection on a single local link.
- Functions:
	- [[Framing Methods|Frames raw bits]] into meaningful chunks (from physical layer to networks layer)
	- Uses MAC addresses to deliver data to the right device on the same network
	- [[Error Control|Error Detection and Correction]]
	- [[Flow Control]] (now in transport layer)
- Protocols:
	- Ethernet
	- PPP (point to point protocol)
	- WiFi
- Devices:
	- Switches
	- Bridges

#### Physical Layer
- Concerned with raw transmission of bits over physical media
- Functions: 
	- Deals with cables, signals, voltages, wireless frequencies
	- Defines connectors, pin layout and transmission rates
- Protocols / Tech:
	- Ethernet cable
	- Fiber optics
	- WiFi signals
	- Bluetooth
- Devices:
	- Hubs
	- Repeaters
	- Cables

For instance =>
When we open a website
- Application -> Browser requests web page using HTTP/S
- Presentation -> Data encryption, compression and translation with SSL/TLS etc
- Session -> Session started between the browser and server
- Transport -> TCP ensures reliable delivery of packets
- Network -> IP decides where the packet should go in the network
- Data link -> Within a local network, ensures delivery to the correct MAC Address. Frame the raw bits
- Physical -> Bits travel as electrical signals

| Name         | Function                                               | Protocols                                 | PDU (Protocol Data Unit) |
| ------------ | ------------------------------------------------------ | ----------------------------------------- | ------------------------ |
| Application  | Interaction with User                                  | HTTP, HTTPS, SMTP, FTP, DNS               | Data                     |
| Presentation | Encoding, Decoding, Compression, Translation           | JPEG, ZIP, PDF, SSL, TLS                  | Data                     |
| Session      | Establishing, maintaining and ending of session        | netBIOS, RPC                              | Data                     |
| Transport    | End to end reliable communication                      | TCP/IP, UDP                               | Segment, Datagram        |
| Network      | Logical routing                                        | IP, BGP, RIP                              | Packet                   |
| Data Link    | Framing, Physical routing, Error control, Flow control | Ethernet (MAC), ARP                       | Frame                    |
| Physical     | Sending of raw bits                                    | Ethernet (physical), USB, bluetooth, wifi | Bits                     |


[[Network Devices]]