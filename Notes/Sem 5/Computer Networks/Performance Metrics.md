# Delay
---

#Transmission-Delay
- time to put the data from device to wire
- $TD = \frac{message size}{band width}$
- Maximum size of message is 1514B or 1522B based on header
#Propagation-Delay
- time taken to send the message through the wire
- $PD = \frac{distance}{speed}$
#Queuing-Delay
- how much time it waits in the buffer
- Negligible
#Latency 
- $L = TD + PD + QD$
# Loss
---
>[! Where loss can happen]
>- generation
>- transmission
>- transit

>[!Reasons for loss]
>- internet disconnection
>- physical tampering
>- Conjuction when multiple nodes are connected to a router

- Transport layer will check the loss of data
- Loss can be identified from the headers of the message

>[!Types of medium]
>- Guided -> No possibility of loss
>- Unguided

#### Relation b/w [[Networking Concepts#Band Width|Bandwidth]], [[Networking Concepts#Throughput|Throughput]] and [[Networking Concepts#Input Traffic|Input Traffic]]
---
Throughput <= Bandwidth (ideally)
but it can temporarily exceed, resulting in congestion or packet loss

Practically Throughput < Bandwidth
Throughput can reduce drastically when collisions occur
