Flow control is the process of regulating the amount of data the sender can transmit before waiting for acknowledgement, so that the receiver is not overwhelmed. 

> [!question] Why needed?
> - Sender can be fast (powerful machine or fast network)
> - Receiver can be slow (weaker machine or limited buffer memory)
> - If sender keeps transmitting without control -> receiver's buffer overflows -> frame loss

- So flow control ensures the synchronization between sender and receiver speeds

![[Flow Control vs Conjuction Control.canvas|Flow Control vs Conjuction Control]]
Here, the control between (S1 and S4), (S2 and S4), (S3 and S4) are all flow control. But the control between (R2 and R4) is not flow control, its conjunction control (Network layer)

Flow control techniques are:
- Stop and Wait ARQ
- Sliding Window
	- Go-back-N ARQ
	- Selective Repeat ARQ

>[!info] ARQ stands for Automatic Repeat Request
#### Stop and Wait ARQ
- The window size is 1 for both sender and receiver
- i.e., after sending a frame, it waits for acknowledgement and sends the second frame only after receiving the acknowledgement
- If acknowledgement didn't arrive within the timer, the frame is re-transmitted.
- Sequence number is kept to prevent duplicate frames or acknowledgements

3 situations:
- Acknowledgement is lost
	- Sender sends Frame 0, receiver gets it correctly, delivers it to network layer and sends ACK 1.
	- ACK 1 is lost in the channel
	- Timer expires, and sender re-transmits Frame 0.
	- Receiver gets Frame 0. but since it is already processed it, it discards the duplicate 
	- Receiver still sends ACK 1 again
	- Sender receives ACK 1 and moves on to Frame 1
- Data is lost
	- Sender sends Frame 0 and it gets lost in the channel
	- After the timer expires, since sender doesn't get any ACK, it re-transmits Frame 0.
	- This time, receiver receives Frame 0 and sends ACK 1 back.
- Delayed acknowledgement
	- Sender sends Frame 0 -> Receiver gets it and sends ACK 1
	- ACK 1 is delayed in the channel.
	- Timer expires (it thinks, ACK is lost) => Sender re-transmits Frame 0.
	- Receiver receives Frame 0 again, finds that it is duplicate and discards and send ACK 1 again.
	- Sender gets ACK 1 (whether the first delayed one or the second one), and moves to the next frame.

For this method, the sequence number will be 0 and 1 only, i.e., Frame 0 -> Frame 1 -> Frame 0 -> Frame 1 ...
##### Advantages
- Very easy to implement
- Minimal buffer required (just one frame at a time)
##### Disadvantages
- Inefficient use of channel
- Band width wastage
- Poor performance in noisy channels

#### Go-back-N ARQ
- sender is allowed to transmit multiple frames (N) before needing an acknowledgement
- sender window size is N
- receiver window size is 1: receiver only accepts the next expected frame, discard others
- receiver accepts the frames ==in order== 
- If it detects an error or a missing frame, it discards that frame and all the subsequent frames (even if they are correct)
- It then waits for the re-transmission of the missing frame
- Sends cumulative acknowledgement.
- If receiver send ACK 5, it means, all the frames up to 5 have been received correctly

Frame list of the sender will contains frames of 3 types
- Frames which are not sent
- Frames which are sent but not acknowledged
- Frames which are sent and acknowledged
##### Advantages
- Much better efficiency than Stop-and-Wait
- Sender can keep the channel busy by transmitting multiple frames
##### Disadvantages
- Wastage due to re-transmission: If one frame is lost, all the subsequent frames in the window has to be re-transmitted even if they are received correctly.
- Receiver can handle frames in order only.

>[!failure] Practice few questions

#### Selective Repeat
- Similar to Go-back-N.
- Only difference is that, only the lost frames will be re-transmitted instead of re-transmitting all the frames after the lost one
- Can accept out of order frames and buffer them until missing frames arrive
- Sends individual acknowledgements (not cumulative)
- sender and receiver window size are same = N / 2 each
#### Advantages
- Efficient usage of band width
- Better performance than Go-back-N when error rate is high
- Receiver can handle out-of-order frames
#### Disadvantages
- More complex receiver: needs buffering and reordering
- Larger memory required for receiver: must store out-of-order frames
- sender and receiver window size should be carefully managed to avoid ambiguities

>[!info] Why window size is $2^k / 2$ each
>If k = 3 (number of bits for sequence number)
>sequence numbers will be -> 0, 1, 2, 3, 4, 5, 6, 7, 0, 1, 2, ...
>suppose the window size is 6 which is larger than 2^k / 2
>
>- suppose, sender sends frames 0 to 5,
>- they are correctly received and acknowledged
>- Window slides -> next frames are (6, 7, 0, 1, 2, 3)
>- But if the old frame 0 is still in the network, the receiver cannot tell whether its the old 0 or the new 0