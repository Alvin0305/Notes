- provides service to the network layer
	- unacknowledged connection less service(Ethernet)
		- Error rate is very low
		- No need of acknowledgement
	- acknowledged connection less service(WiFi)
	- acknowledged connection oriented service(Satellite communication)
		- Long distance
		- Error rate is very high
		- Acknowledgement is required
- utilizing the services / functioning in the link layer by getting the information from physical layer
	- [[Error Control]]
	- [[Framing Methods|Framing]] (separate header, payload and trailer)
	- [[Flow Control]]

#### Connection Oriented
---
- Dedicated logical connection is established before data transfer. 
- Frames follow the connection in order
- Error correction and recovery is guaranteed by link layer.
- For a satellite communication, before sending the frames, sender and receiver agree on communication (hand shake). And each frame is acknowledged to make the communication reliable as it is long distance and is error prone

>[!info] Steps involved
>- Connection establishment
>- Send or receive data
>- Connection release

>[!example]
>There are 4 devices, S1, S2, S3 and S4. S1 and S2 wants to communicate with S3 => S3 allocates dedicated resources for S1 and S2 resulting in a connection establishment to start communication. Consider, S3 have resource only for handling 2 devices at a time => When S4 tries to establish a connection, S3 declines until S1 or S2 releases the connection

#### Connection less
---
- No dedicated path is set up between sender and receiver
- Each frame is treated independently
- No guarantee of delivery, ordering or error recovery (unless handled in higher layers)

>[!info] Steps involved
>- Send or receive data
>  
>i.e., there is no connection establishment or connection release
