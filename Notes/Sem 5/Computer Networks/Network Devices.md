- **Hub**
- **Bridge**
- **Switch**
- **Router**
#### Hub
- A very basic device that connects multiple computers in a device.
- Takes the incoming data from one port and broadcasts it to all the other ports, regardless of the destination.
- Works at Layer 1 (Physical layer) only => No knowledge on MAC address => forwards everything to every node.
- Uses half duplex => cannot send and receive data at the same time.
- Drawbacks:
	- Causes collision.
	- Not intelligent, no filtering.
#### Bridge
- A device that connects and filters traffic between two LAN segments
- Works at layer 2 (Data link layer), just like switch => Has MAC Address.
- Uses MAC address to forward traffic to the correct segment.
- Can be used to divide large collision domain into smaller ones, improving performance.
- Unlike Hub, it doesn't broadcast everything, it decides whether to forward or filter frames based on the destination MAC address.
#### Switch
- Smarter than hub, forwards data only to the specific device (MAC address) it is meant for.
- Works at Layer 2 (Data link layer) => It have MAC address => forwards message to the recipient only.
- Maintains a MAC address table to know which device is connected to which port.
- No collisions: by creating dedicated paths => No collision domain.
- If destination MAC address is FF.FF.FF.FF.FF.FF, then the message will be broadcast-ed to everyone => Broadcast domain.
#### Router
- Connected difference networks together and routes packets based on the IP address
- Works at Layer 3 (Network layer) => Has MAC address and IP address
- Can assign IPs ([[Networking Concepts#DHCP Server|DHCP]]), provide [[Networking Concepts#NAT|NAT]] (translate private IPs to public IPs and vice versa)
- Used to connect LAN to internet
### **Network Domains**
#### Collision-Domain
- Its like a one-lane hallway => at a time only one man can walk through it. If more than one walks, collision occurs.
#### Broadcast-Domain
- Its like a single room => If a person shouts, all others in the room has to stop and listen even though its not for them.

> **Hub**
> - All devices connected to the hub are in the same one-lane hallway. When one PC sends a signal, all other PCs should wait. If not, the data packets collide. Makes the network slow and inefficient => A large collision domain
> - When a PC connected to the hub sends a message, it will be send to all other PCs connected to it irrespective to the receiver. => A large broadcast domain

> **Bridge**
> - Divides a large collision domain into smaller ones by segmenting the network
> - Still all devices connected via bridge remain in the same broadcast domain
> - Example: If you connect two hubs using a bridge, each hub will be its own collision domain, but both hubs still share one broadcast domain

> **Switch** 
> - Each port on a switch is its own private one-lane hallway to the device connected to it. Therefore multiple PCs can communicate simultaneously without collision
> - If the recipient is unknown, the message is broadcast-ed to all the devices connected to it => A large broadcast domain

> **Router**
> - Similar to switch: each port in a router has its own collision domain. There won't be collision between devices connected to different ports of the router
> - Messages received will be sent to the target port only, not broadcast-ed to all ports. A router be default does not forward a broadcast message. 

|                  | Hub | Switch | Bridge | Router |
| ---------------- | :-: | :----: | :----: | :----: |
| Collision Domain |  ✔  |   ✘    |   ✘    |   ✘    |
| Broadcast Domain |  ✔  |   ✔    |   ✔    |   ✘    |
