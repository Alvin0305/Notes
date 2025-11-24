Topology refers to: how the devices are connected.
This includes
- Physical connection
- Logical connection 
- Hybrid (Both physical and logical)

## Bus Topology
---

![[Bus.png]]

- When a message is sent, all the intermediate nodes receives the message but will discard it.
- At the ends of the bus, there will be a terminator node which prevents the bounce back of signals
#### Advantages
- Easy to implement
- Cost efficient for small networks
#### Disadvantages
- Collisions
- If the bus fails, entire network goes down
- Troubleshooting is difficult

## Star Topology
---

![[Star.png]]

- The device in the center would be a hub or a switch
- The hub acts as a controller
#### Advantages
- When one device fails, network remains intact
- Easy to install and manage
- Easy to add and remove devices
#### Disadvantages
- When the central hub fails, the entire network fails
- Require more cables compared to bus

## Ring Topology
---
![[Ring.png]]

- Each device is connected to exactly two others, forming a circle
- Data travels in one direction(unidirectional) or both(bidirectional)
#### Advantages
- Simple and easy to implement
#### Disadvantages
- If one device fails, the entire network fails
- Adding or removing a device disrupts the network
- Troubleshooting is hard

## Token Ring
---
![[Token Ring.png]]

- A token (control frame) circulates around the ring
- Only device holding the token can transmit
- If a device receives a message with token, it checks the address and forwards.
- When the message receives the address, it attaches a flag and forwards
- When the message comes back to the sender with the flag, it removes the flag and the message and forwards the token, which the next device can use.
#### Advantages
- No collisions
- Predictable performance under heavy load also
#### Disadvantages
- More complex
- Slower compared to modern Ethernet
- Token loss or duplication can cause issues

## Mesh Topology
---
![[Mesh.png]]

- Every machine is connected to every other machine
- For n nodes, $\frac{n(n-1)}{2}$ connections are needed
#### Advantages
- Very reliable
- High fault tolerance
- Supports high traffic (multiple paths)
#### Disadvantages
- Expensive
- Complex installation and maintenance
- Not scalable for a very large network

## P2P Topology
---
![[P2P.png]]

- Point to Point topology
- Direct connection between two nodes only
- Can be wired or wireless
#### Advantages
- Very simple, minimal configuration
- Maximum bandwidth dedicated to the link
- Very reliable for two-device communication
#### Disadvantages
- Only connected two nodes (not scalable)

---
NSL is physically STAR topology
But logically its Bus or Ring (inside the switch)
=> Its Hybrid

>[!note]
>Hybrids are of 2 types
>- STAR + Bus
>- STAR + Ring

