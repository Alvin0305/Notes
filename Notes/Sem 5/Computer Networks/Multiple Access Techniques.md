- When multiple communications happen simultaneously, others messages can interfere with our communication. Such interference is called noise. To avoid such noise, we use the following techniques
#### Code Division Multiple Access (CDMA)
- All users use the same frequency and time, but separated by unique codes
- Used in 2G/3G mobile systems and military communications

In this method, we keep a chip sequence. To send a '1' bit, we send this chip sequence, to send a '0' bit, we send the complement of the chip sequence. 
To transmit 1 bit, we need to transmit 8 bits (practically 64 or 128 bit chip sequence)

>[!example]
>Consider there are 4 systems A, B, C and D
The chip sequence of each are
>```
A = 00011011
B = 00101110
C = 01011100
D = 01000010
>```
>
>In CDMA, 
>- 1 means '1' bit transmission
>- -1 means '0' bit transmission
>- 0 means no transmission
>Therefore, these chip sequences can be rewritten as
>```
>A = -1 -1 -1 1 1 -1 1 1
>B = -1 -1 1 -1 1 1 1 -1
>C = -1 1 -1 1 1 1 -1 -1
>D = -1 1 -1 -1 -1 -1 1 -1
>```
>At time t1, let C transmit '1'
>`S1 = C = -1 1 -1 1 1 1 -1 -1`
>At time t2, let B and C transmit '1'
>`S2 = B + C = -2 0 0 0 2 2 0 -2`
>At time t3, let A transmit '1' and B transmit '0'
>`S3 = A + B' = 0 0 -2 2 0 -2 0 2`
>At time t4, let A and B transmit '1' and C transmit '0'
>`S4 = A + B + C' = -1 -3 1 -1 1 -1 3 1`
>
>Consider, I am D and I am communicating with C, and all other signals are just noise to me. I just wanna know what C is speaking. For that, we take dot product of the signal I receive with C's chip sequence
>At time t1, 
>`S1.C = (1 + 1 + 1 + 1 + 1 + 1 + 1 + 1) / 8 = 1` => C transmitted '1'
>At time t2,
>`S2.C = (2 + 0 + 0 + 0 + 2 + 2 + 0 + 2) / 8 = 1` => C transmitted '1'
>At time t3,
>`S3.C = (0 + 0 + 2 + 2 + 0 - 2 + 0 - 2) / 8 = 0` => C transmitted nothing
>At time t4,
>`S4.C = (1 - 3 - 1 - 1 + 1 - 1 - 3 - 1) / 8 = -1` => C transmitted '0'

In this way, we can filter out the required data from the noise
#### Frequency Division Multiple Access (FDMA)
- Each station is given different frequency range.
- Guard band prevents overlapping
- The allowed frequency band will be equally split to all the devices
- Devices can communicate simultaneously
- e.g., 1G
#### Time Division Multiple Access (TDMA)
- Same frequency spectrum is given to all the devices
- But each device is allotted different time interval
- A device can communicate only in its allotted time
- Every station gets the complete bandwidth
- Time is allotted in Round Robin fashion
- e.g., 2G GSM (global system for mobile communication)
#### Statistical Time Division Multiple Access (STDMA)
- Similar to TDMA
- Difference is in the fact that, the time allotted for a device is dynamically calculated based on the traffic for each station
- used in ships
#### Orthogonal Frequency Division Multiplexing (OFDM)
- When one station is using the peak frequency, the other will be at zero
- Used in phones
- Each of the frequency sub-carrier allotted to users are orthogonal to each other. This means, even though they overlap, they doesn't interfere with each other.
- e.g., Wi-Fi, 4G LTE, 5G NR
#### Difference between FDMA and OFDM

![[OFDM vs FDMA.png]]

