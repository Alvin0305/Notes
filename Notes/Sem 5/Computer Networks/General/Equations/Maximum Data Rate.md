Maximum data rate depends on whether the channel is noiseless or noisy. 

#### Noiseless Channel

**Nyquist Bit Rate Formula**
For a noiseless channel with bandwidth B Hz and using V signal levels. 
$$
\text{Maximum Data Rate} = 2B\log_2(V) \text{ bits/s}
$$

#### Noise Channel

**Shannon Capacity Formula**
For a channel with bandwidth B Hz and signal-to-noise ratio (S/N):
$$
\begin{aligned} 
\text{Maximum Data Rate} &= B\log_2(1 + \frac{S}{N}) \\
SNR &= 10\log_{10}(\frac{S}{N}) \\
\implies \frac{S}{N} &= 10 ^ \frac{SNR}{10}
\end{aligned}
$$
- S/N -> Signal to Noise Ratio