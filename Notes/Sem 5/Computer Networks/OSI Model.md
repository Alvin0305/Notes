- Application Layer
- Presentation Layer
- Session Layer
- Transport Layer
- Network Layer
- Data link Layer
- Physical Layer

### Application Layer
---

- Provide network services directly to the user/application
- example: Web Browsers, email clients, file transfer apps
- Protocols: 
	- HTTP
	- HTTPS
	- SMTP
	- FTP
	- DNS

#### Presentation Layer
---

- Translates, Compresses or encrypts data so application can understand it
- Functions:
	- Data format conversions -> JPEG, MP4, PDF
	- Encryption, Decryption -> SSL, TLS
	- Compression -> ZIP, MPEG

#### Session Layer
---

- Manages session between two systems
- Functions:
	- Establishing, maintaining and ending sessions
	- Synchronization in data transfer
- Protocols:
	- NetBIOS
	- RPC

#### Transport Layer
---

- Ensure end-to-end communication with reliability and error-checking
- Functions:
	- Split data into Segments
	- Re-transmit if data is lost
	- Error detection
	- Can be connection oriented(TCP) or connection less(UDP)
- Protocols
	- TCP
	- UDP

#### Networks Layer
---

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
---

- Handles physical addressing (MAC) and error detection on a single local link.
- Functions:
	- Frames the raw bits into meaningful chunks (from physical layer to networks layer)
	- Uses MAC addresses to deliver data to the right device on the same network
	- Error detection with checksums
- Protocols:
	- Ethernet
	- PPP
	- WiFi
- Devices:
	- Switches
	- Bridges

#### Physical Layer
---

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

[[Network Devices]]