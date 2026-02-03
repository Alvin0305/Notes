#### Bandwidth Delay Product
$$\text{Bandwidth Delay Product} = Bandwidth \times \text{Delay}$$
$$\text{Bandwidth Delay Product} \approx Bandwidth \times \text{Round Trip Time}$$
- This is the maximum number of bits which can be held in link.
- Here Delay is approximately equal to RTT because, for RTT, we consider message of very small size
#### Delay

#Transmission-Delay
- time to put the data from device to wire
$$TD = \frac{message size}{band width}$$
- Maximum size of message is 1514 B or 1522 B based on header

#Propagation-Delay
- time taken to send the message through the wire
$$PD = \frac{distance}{speed}$$

#Queuing-Delay
- how much time it waits in the buffer
- Negligible

#Latency 
$$L = TD + PD + QD$$
#RTT
- Round Trip Time
- Time for a packet to go from sender → receiver → back to sender
- The packet will be very small such that its transmission delay can be neglected
$$\text{Round Trip Time} \approx 2 \times \text{Propagation Delay}$$
Example:
- Ping command measures RTT
Used heavily in TCP for timeout calculations.
#### Loss

>[! Note]
>Where loss can happen
>- generation
>- transmission
>- transit

>[!Note]
>Reasons for loss
>- internet disconnection
>- physical tampering
>- Conjuction when multiple nodes are connected to a router

- Transport layer will check the loss of data
- Loss can be identified from the headers of the message

>[!Note]
>Types of medium
>- Guided -> No possibility of loss
>- Unguided
#### Relation b/w [[Networking Concepts#Band Width|Bandwidth]], [[Networking Concepts#Throughput|Throughput]] and [[Networking Concepts#Input Traffic|Input Traffic]]
Throughput <= Bandwidth (ideally)
but it can temporarily exceed, resulting in congestion or packet loss

Practically Throughput < Bandwidth
Throughput can reduce drastically when collisions occur
#### Utilization (Efficiency)
$$Efficiency = \frac{\text{Actual used Bandwidth}}{\text{Total available bandwidth}} \times 100$$

