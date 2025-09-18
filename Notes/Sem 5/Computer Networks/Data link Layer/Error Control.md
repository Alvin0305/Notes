Errors can happen if the data being send gets corrupted, or the data or the resulting acknowledgement is not received. 
To ensure that all messages are properly send and acknowledged

for every frame we send (*F1*), we start a timer. If we don't get an acknowledgement within the timer, we resend it (*F2*).

- Situation 1 => Data is not received
	- So, after the timer, sender resend the frame (F2). So when the receiver get F2, it checks the sequence number and finds that it is the expected one and takes it and send a positive acknowledgement.
- Situation 2 => Data is properly received, but acknowledgement is lost
	- So, after the timer, sender resend the frame (F2). So when the receiver get F2, it checks the sequence number and finds that, it has already been received. Therefore it discards F2 but sends a positive acknowledgement 

Transmission Errors
1. Single bit error
2. Bursty error => multiple bits (consecutive or not) caused by unreliable links

>[!tip] 
>- For guided media, we use error detection only, error correction is not necessary as error occurs rarely. If an error occurs, it acknowledge the sender to re-transmit. 
>- For unguided media, we use error correction. Because, error occurs more frequently than guided media. Cost of re-transmission should be taken into account. 
### Error Correction Methods
#### CRC (Cyclic Redundancy Code)
- CRC is a error-detecting code based on polynomial division
- sender and receiver agrees on a generator polynomial G(x)
- Receiver performs some calculation: if the remainder is 0 => no error, else => error detected

- CRC can detect 
	- single bit errors
	- double bit errors
	- burst error => upto length of the generator polynomial

- M(x) = message bits (data) represented as a polynomial
- G(x) = generator polynomial (known to both sender and receiver)
- R(x) = remainder (CRC bits to append)

What sender does?
1. Take the message bits: M
2. Append (degree of G(x)) zeroes to the end of the message
3. Divide the extended message by the generator polynomial G(x) (binary division using XOR)
4. Get the remainder R
5. transmit M + R

What receiver does?
1. Receives M + R
2. Divides by the same G(x)
3. If remainder = 0 -> frame is correct
4. If remainder != 0 -> error detected

>[!example]
>Message, M = 101100
>Generator, G(x) = 1101
>=> degree(G(x)) = 3 => 3 degree bits should be added
>=> After appending zeroes -> 101100000
>
>Doing binary division (XOR method)
>Quotient is not needed, Just the remainder is needed
>Remainder = 0111
>=> R = 111
>=> Transmitted frame = M + R = 101100111
>
>In receiver side, 
>101100111 is divided by 1101 => Remainder = 0000 => No error
#### Hamming Distance Code
- Parity bits are placed in specific positions so that the receiver can locate and correct errors. 
- Data bits + Redundant (parity) bits = Hamming Code word
- Parity bits are placed at positions that are powers of 2
- each parity bit checks some subset of bits

>[!question] How to find the number of parity bits needed?
>if m = number of data bits, and r = number of parity bits, r can be found by
>$2^r \ge m + r + 1$
>
>m + r = total length of code word

>[!example]
>to send 4 bits of data
>```
>if r = 1, 
>2 < 4 + 1 + 1
>if r = 2,
>4 < 4 + 2 + 1
>if r = 3,
>8 <= 4 + 3 + 1
>```
>therefore, r = 3
>=> sizeof code word = m + r = 4 + 3 = 7

```
Bit positions: 1  2  3  4  5  6  7 
Bit Types:     P1 P2 D1 P3 D2 D3 D4
```

Here, P1, P2 and P3 are parity bits, which we need to find out. D1, D2, D3 and D4 are the data bits which we have in hand

```
data = 1011
Bit positions: 1  2  3  4  5  6  7 
Bit Types:     P1 P2 1  P3 0  1  1
```

Now we need to find P1, P2 and P3, for that, first we need to find which all positions are controlled by each parity bit, for that, we need to write the binary form of each index

```
Bit positions:  1    2    3    4    5    6    7
Binary form:   0001 0010 0011 0100 0101 0110 0111
```

From this, we can see that, 
- 1, 3, 5 and 7 have 1 in the last position
- 2, 3, 6 and 7 have 1 in the second last position
- 4, 5, 6 and 7 have 1 in the third last position

Here we will be using Even parity
=> the number of ones in each of the above 3 groups should be even
i.e., for first group (1, 3, 5, 7) => _ + 1 + 0 + 1 should be even => the parity bit (1st index) should be 0
similarly, in second group (2, 3, 6, 7) => _ + 1 + 1 + 1 should be even => the parity bit (2nd index) should be 1
similarly, in third group (4, 5, 6, 7) => _ + 0 + 1 + 1 should be even => the parity bit (4th index) should be 0

Therefore, the code word is => 0110011
This code word will be transmitted to the receiver.
The code word is unique for a given data

>[!question] 
>How the receiver checks and correct errors?
>
Consider the situation, due to some issue, instead of 0110011, 0110111 is received
>
>The receiver will check if all the 3 groups add of even parity
>for first group (1, 3, 5, 7) => 0 + 1 + 1 + 1 => 3 => odd => error
>similarly, for second group (2, 3, 6, 7) => 1 + 1 + 1 + 1 => 4 => even
>and, for third group (4, 5, 6, 7) => 0 + 1 + 1 + 1 => 3 => odd => error
>
>From this we can say there is an error, to identify at which position error occurred, we add up the parity bit position which have errors => 1 + 4 = 5 
>i.e., error is in the 5th position, so we can toggle the bit and correct the error

>[!info] 
>Minimum Hamming distance (d)
>
>- Minimum Hamming distance is the minimum value obtained by doing XOR in all possible pairs of code words.
>- Hamming Code can detect d - 1 bit errors and correct $\frac{d-1}{2}$ bit errors

>[!warning] Hamming Code is not always correct
>Hamming code can give false positives, i.e., it may say a code is correct even though it might have some errors. 
>Solution is to use Read Solomon Codes or use LDPC (Low density parity check) or by adding one more parity bit
