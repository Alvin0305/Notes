Errors can happen if the data being send gets corrupted, or the data or the resulting acknowledgement is not received. 
To ensure that all messages are properly send and acknowledged

for every frame we send (*F1*), we start a timer. If we don't get an acknowledgement within the timer, we resend it (*F2*).

- Situation 1 => Data is not received
	- So, after the timer, sender resend the frame (F2). So when the receiver get F2, it checks the sequence number and finds that it is the expected one and takes it and send a positive acknowledgement.
- Situation 2 => Data is properly received, but acknowledgement is lost
	- So, after the timer, sender resend the frame (F2). So when the receiver get F2, it checks the sequence number and finds that, it has already been received. Therefore it discards F2 but sends a positive acknowledgement 
