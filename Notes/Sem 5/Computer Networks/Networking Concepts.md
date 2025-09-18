#### NAT
- Network Address Translation
- all normal devices have a private IP. With a private IP, we cannot access the internet. So, we use a NAT (in Router) which converts our private IP to the router's public IP. Using this public IP, we can communicate in the internet.
- It converts both private to public as well as public to private
#### Subnet mask
- Each IP address contains two parts: 
	- Network address
		- The part which identifies, which local network you belong to 
	- Host address
		- The part which identifies the specific device within the network
- example: 10.20.30.40/16
	=> 16 is the subnet mask i.e., 255.255.0.0 (16 bits will be 1 and rest will be 0)
	=> first 16 bits is the network address => 10.20.0.0
	=> rest 16 bits is the host part => 30.40
	=> All the device in the network will have address in the form 10.20.x.x
#### DHCP Server
- Dynamic Host Configuration Protocol 
- IPs can be assigned statically or dynamically. 
- Static IP means, we assign the IP for a device on our own, this includes the IP address, subnet mask and default gateway. This should be done carefully so that, there is no two devices in the same network having the same IP address. Which is tiresome.
- Therefore we use a dedicated server for that, called DHCP server which dynamically assign these values for each device connected to that network. Also, IPs are assigned in lease, i.e., the IP assigned to a device will be revoked after a period of time to prevent IP range exhaustion (A DHCP server will be given only a range of IPs to be assigned). There is option to assign the same IP to a device all the time by maintaining a address reservation table in DHCP server (mainly for devices like printer, routers, servers etc)
#### Band Width
- It is the maximum theoretical capacity of a channel
- Unit is Mbps, Gbps...
#### Throughput
- Actual data rate achieved over the network
- Utilization of the bandwidth
- Unit is same as Bandwidth
#### Input Traffic
- The data offered to the network (traffic entering the network)
- Unit is same as Bandwidth