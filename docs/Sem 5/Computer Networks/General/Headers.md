## 🧩 1. IEEE 802.3 / Ethernet Frame Header (Data Link Layer)

An Ethernet frame consists of:

| Field                       | Size          | Purpose                                               |
| --------------------------- | ------------- | ----------------------------------------------------- |
| **Preamble + SFD**          | 8 bytes       | Synchronization (sender → receiver lock)              |
| **Destination MAC Address** | 6 bytes       | Receiver NIC address                                  |
| **Source MAC Address**      | 6 bytes       | Sender NIC address                                    |
| **Length/Type**             | 2 bytes       | Payload length or protocol type (e.g., IPv4 = 0x0800) |
| **Payload/Data**            | 46–1500 bytes | Actual network layer packet                           |
| **CRC / FCS**               | 4 bytes       | Error detection                                       |

📌 Minimum payload = 46 bytes → If smaller, **padding** is added to meet 64-byte minimum frame.
#### Ethernet Exam Quick Notes

- **Frame format has MAC addresses (not IP).**    
- **Minimum frame size = 64 bytes, maximum = 1518 bytes.**
	- Excluding the preamble, 6 + 6 + 2 + 4 = 18
	- Therefore, minimum size of payload = 64 - 18 = 46B
- CRC detects errors but **cannot correct** them.
- Protocol type determines whether the packet is:
	- IPv4
	- IPv6
	- ARP
	- VLAN Tagging...
- Preamble + SFD => 62 bits of '10' and at the end '11'
	- It is used to sync the communication
## 🌍 2. IPv4 Header (Network Layer)

IPv4 header has **variable size**: minimum **20 bytes**, max **60 bytes** (due to Options field).

| Field                  | Size     | Meaning                                            |
| ---------------------- | -------- | -------------------------------------------------- |
| Version                | 4 bits   | Always **4** for IPv4                              |
| IHL                    | 4 bits   | Header length in 32-bit words (min = 5 → 20 bytes) |
| DSCP / Type of Service | 8 bits   | QoS / priority                                     |
| Total Length           | 16 bits  | Entire packet = header + data                      |
| Identification         | 16 bits  | Used for fragmentation                             |
| Flags                  | 3 bits   | Fragmentation control (DF / MF)                    |
| Fragment Offset        | 13 bits  | Position of fragment                               |
| TTL                    | 8 bits   | Prevents loops (each router decrements)            |
| Protocol               | 8 bits   | Next layer (TCP=6, UDP=17, ICMP=1)                 |
| Header Checksum        | 16 bits  | Error detection for header                         |
| Source IP              | 32 bits  | Sender logical address                             |
| Destination IP         | 32 bits  | Receiver logical address                           |
| Options                | variable | Rarely used                                        |
| Padding                | variable | To maintain 32-bit alignment                       |
#### Explanation
##### First 32-bit Word (4 Bytes)

**1. Version (4 bits)**
- **Purpose:** Identifies the IP version
- **Value:** Always `0100` (binary) for IPv4, which is `4` in decimal
- **Example:** `4`

**2. Header Length (IHL) (4 bits)**
- **Purpose:** Specifies the header length in **32-bit words**
- **Calculation:** Minimum value is `5` (means 5 × 4 = 20 bytes), maximum is `15` (15 × 4 = 60 bytes)
- **Example:** `5` (for a typical 20-byte header with no options)

**3. Differentiated Services Code Point (DSCP) (6 bits)**
- **Purpose:** Used for Quality of Service (QoS) to prioritize certain types of traffic
- **Values:** Can mark traffic for different treatment (e.g., voice, video, best-effort)
- **Example:** `0` (default, best-effort service)
    
**4. Explicit Congestion Notification (ECN) (2 bits)**
- **Purpose:** Allows end-to-end notification of network congestion without dropping packets
- **Values:** `00` = Non-ECN-capable, `01` or `10` = ECN-capable, `11` = Congestion experienced
- **Example:** `00` (not using ECN)

**5. Total Length (16 bits)**
- **Purpose:** Total length of the IP packet (header + data) in bytes
- **Range:** Minimum 20 (header only), maximum 65,535 bytes
- **Example:** `60` (20 bytes header + 40 bytes data)

##### Second 32-bit Word

**6. Identification (16 bits)**
- **Purpose:** Uniquely identifies a group of fragments of a single IP datagram
- **Usage:** All fragments of the same original packet have the same Identification value
- **Example:** `0x4d21` (a random number assigned by the sender)

**7. Flags (3 bits)**
- **Bit 0:** Reserved (must be 0)
- **Bit 1: Don't Fragment (DF)** - `0` = May fragment, `1` = Don't fragment
- **Bit 2: More Fragments (MF)** - `0` = Last fragment, `1` = More fragments coming
- **Example:** `010` (DF=1, MF=0 - Don't fragment, this is the only/last fragment)

**8. Fragment Offset (13 bits)**
- **Purpose:** Specifies the position of this fragment in the original packet, in 8-byte blocks
- **Usage:** Used to reassemble fragments in the correct order
- **Example:** `0` (no fragmentation, or this is the first fragment)

##### Third 32-bit Word

**9. Time to Live (TTL) (8 bits)**
- **Purpose:** Prevents packets from circulating forever in routing loops
- **Mechanism:** Decremented by 1 at each router. Packet is discarded when TTL reaches 0
- **Typical Values:** `64` (Linux/Unix), `128` (Windows), `255` (maximum)
- **Example:** `64`

**10. Protocol (8 bits)**
- **Purpose:** Identifies the protocol in the payload portion of the IP datagram
- **Common Values:**
    - `6` = TCP
    - `17` = UDP
    - `1` = ICMP
    - `2` = IGMP
- **Example:** `6` (TCP)

**11. Header Checksum (16 bits)**
- **Purpose:** Error-checking for the IP header only (not the data)
- **Mechanism:** Calculated by the sender and verified by each router. If invalid, packet is discarded
- **Example:** `0x7a9d` (a calculated checksum value)

##### Fourth 32-bit Word

**12. Source IP Address (32 bits)**
- **Purpose:** IP address of the sender
- **Format:** 4 octets in dotted-decimal notation
- **Example:** `192.168.1.10`

##### Fifth and Sixth 32-bit Words

**13. Destination IP Address (32 bits)**
- **Purpose:** IP address of the intended recipient
- **Format:** 4 octets in dotted-decimal notation
- **Example:** `8.8.8.8` (Google DNS)

##### Optional Fields

**14. Options (Variable length, 0-40 bytes)**
- **Purpose:** Optional fields for special handling (rarely used in practice)
- **Must be padded** to make the total header length a multiple of 4 bytes
- **Common Options:** Record Route, Timestamp, Security, etc.

📌 Properties to memorize:
- Designed for fragmentation → **Identification + Flags + Fragment Offset**
- **TTL decreases at routers**, packet discarded if 0
- **Checksum exists only in IPv4**, not IPv6
## 🌐 3. IPv6 Header (Network Layer)

IPv6 was made simpler and faster than IPv4.

🟢 **IPv6 header is fixed 40 bytes**

| Field          | Size     | Meaning                                               |
| -------------- | -------- | ----------------------------------------------------- |
| Version        | 4 bits   | Always **6**                                          |
| Traffic Class  | 8 bits   | QoS                                                   |
| Flow Label     | 20 bits  | Identify a flow (e.g., video stream)                  |
| Payload Length | 16 bits  | Size of data after the header                         |
| Next Header    | 8 bits   | Protocol of next header (TCP/UDP) or extension header |
| Hop Limit      | 8 bits   | Equivalent of TTL                                     |
| Source IP      | 128 bits | Sender address                                        |
| Destination IP | 128 bits | Receiver address                                      |
#### Explanation

##### 1. Version (4 bits)

- **Purpose:** IP protocol version    
- **Value:** Always `0110` (binary) for IPv6, which is `6` in decimal
- **Example:** `6`

##### 2. Traffic Class (8 bits)
- **Purpose:** Similar to IPv4's DSCP field - used for QoS packet prioritization
- **Usage:** Can mark traffic for different classes of service (e.g., voice, video, background)
- **Example:** `0` (default best-effort)

##### 3. Flow Label (20 bits)

- **Purpose:** **New in IPv6** - Identifies packets that belong to the same "flow" or conversation 
- **Usage:** Routers can identify related packets and handle them consistently (same path, same QoS)
- **Example:** `0x4D21A` (a random flow identifier)

##### 4. Payload Length (16 bits)

- **Purpose:** Length of the **data** following the IPv6 header (includes extension headers) 
- **Key Difference:** In IPv4, "Total Length" included the header itself. In IPv6, Payload Length counts only what comes **after** the 40-byte header.
- **Range:** 0 - 65,535 bytes
- **Example:** `1280` (typical MTU size minus 40-byte header)

##### 5. Next Header (8 bits)

- **Purpose:** Similar to IPv4's Protocol field, but more powerful 
- **Usage:** Identifies the type of header immediately following the IPv6 header
- **Common Values:**
    - `6` = TCP
    - `17` = UDP
    - `58` = ICMPv6
    - `43` = Routing Header
    - `44` = Fragment Header
    - `0` = Hop-by-Hop Options
- **Example:** `6` (TCP)

##### 6. Hop Limit (8 bits)

- **Purpose:** Same as IPv4's TTL - prevents infinite routing loops 
- **Mechanism:** Decremented by 1 at each router, packet discarded when reaches 0
- **Typical Values:** `64` (common default), `255` (maximum)
- **Example:** `64`

##### 7. Source Address (128 bits)

- **Purpose:** IPv6 address of the sender 
- **Size:** 128 bits (16 bytes) - massively larger than IPv4's 32 bits
- **Example:** `2001:0db8:85a3:0000:0000:8a2e:0370:7334`

##### 8. Destination Address (128 bits)

- **Purpose:** IPv6 address of the intended recipient 
- **Size:** 128 bits (16 bytes)
- **Example:** `2606:4700:4700::1111` (Cloudflare DNS)

📌 Major differences from IPv4:

| IPv4                              | IPv6                    |
| --------------------------------- | ----------------------- |
| Variable header (20–60 bytes)     | Fixed 40 bytes          |
| Supports fragmentation in routers | No router fragmentation |
| Has checksum                      | No checksum             |
| 32-bit address                    | 128-bit address         |
| Options inside header             | Extension headers       |

➡️ IPv6 was designed for **high speed, low processing overhead, large address space**.
## 🚀 4. TCP Header (Transport Layer)

TCP header size **20–60 bytes**.

| Field                 | Size     | Meaning                                    |
| --------------------- | -------- | ------------------------------------------ |
| Source Port           | 16 bits  | Sending application                        |
| Destination Port      | 16 bits  | Receiving application                      |
| Sequence Number       | 32 bits  | Byte number of first byte in this segment  |
| Acknowledgment Number | 32 bits  | Next expected byte from peer               |
| Data Offset           | 4 bits   | Header size                                |
| Reserved              | 3 bits   | For future use                             |
| Flags                 | 9 bits   | SYN, ACK, FIN, RST, PSH, URG, ECE, CWR, NS |
| Window Size           | 16 bits  | Receiver buffer size (flow control)        |
| Checksum              | 16 bits  | Error detection                            |
| Urgent Pointer        | 16 bits  | Used with URG flag                         |
| Options               | variable | MSS, Window Scaling, Timestamp             |

📌 Most important mechanism:

`Sequence number + Acknowledgment number → reliable ordered delivery Window size → flow control (sliding window) Flags → connection setup and teardown (SYN, FIN)`

## ⭐ Putting Layer Headers Together (logical order)

When sending:

```
TCP Segment (app data + TCP header) 
↓ 
IPv4/IPv6 Packet (IP header + TCP segment) 
↓ 
Ethernet Frame (MAC header + IP packet) 
↓ 
Physical transmission (bits)
```

So the structure on the network looks like:

```
| Ethernet |   IP   |   TCP   | Application Data | 
| <----- outer ---->|<--- inner --->|
```

---

# 🧠 Ultimate Memory Table (for last-minute exam recall)

| Header           | Layer     | Address type | Main Purpose                          |
| ---------------- | --------- | ------------ | ------------------------------------- |
| 802.3 / Ethernet | Data Link | MAC (48-bit) | Local delivery (LAN)                  |
| IPv4             | Network   | IP (32-bit)  | Routing between networks              |
| IPv6             | Network   | IP (128-bit) | Routing — scalable & high performance |
| TCP              | Transport | Port numbers | Reliable end-to-end communication     |

---

# 🚀 **5. ARP HEADER (Address Resolution Protocol)**
(Used only on LAN to map IP → MAC)
### **ARP fields**

| Field                       | Size | Meaning            |
| --------------------------- | ---- | ------------------ |
| **Hardware type**           | 2    | Ethernet = 1       |
| **Protocol type**           | 2    | IPv4 = 0x0800      |
| **Hardware Address Length** | 1    | MAC length = 6     |
| **Protocol Address Length** | 1    | IPv4 length = 4    |
| **Opcode**                  | 2    | 1=Request, 2=Reply |
| **Sender MAC address**      | 6    |                    |
| **Sender IP address**       | 4    |                    |
| **Target MAC address**      | 6    |                    |
| **Target IP address**       | 4    |                    |

Total: **28 bytes**

## 🎯 Perfect exam paragraph to write if asked:

> Ethernet (802.3) headers encapsulate data with source and destination MAC addresses for local delivery. IPv4 and IPv6 headers operate at the network layer; IPv4 uses a variable header for fragmentation and routing, whereas IPv6 uses a fixed 40-byte header with a 128-bit address space and extension headers for efficiency. TCP, operating at the transport layer, provides reliable delivery using sequence numbers, acknowledgments, sliding window flow control, and flags such as SYN, ACK, and FIN.
