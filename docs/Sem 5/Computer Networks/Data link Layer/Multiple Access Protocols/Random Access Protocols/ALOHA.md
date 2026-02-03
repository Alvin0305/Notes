## Pure ALOHA

Developed for wireless packet radio networks — **very simple but inefficient**.
### 🔧 Logic
- A device **transmits whenever it has data**
- If a collision occurs → **wait a random time** → retransmit
### 🔥 Vulnerable Period
A packet takes time **T** to transmit.  
Collision can occur if another packet starts **within T before** or **T after** the transmission.

`Vulnerable period = 2T`
### 📌 Throughput
Maximum efficiency (when many users):
$S = G × e^{−2G}$ 
$\text{Max efficiency} = 1 / (2e) ≈ 18.4$

Where:
- G = average number of attempts per packet time
- S = successful throughput
👉 Meaning: Pure ALOHA wastes ~80% of the channel time.

## Slotted ALOHA

Improvement over Pure ALOHA.
### 🔧 Logic
- Time is divided into **equal slots**
- Transmission can start **only at the start of a slot**
- If two users transmit in the same slot → collision
- But vulnerable period becomes **T (not 2T)**
### 📌 Throughput
$S = G × e^{−G}$ 
$\text{Max efficiency} = 1 / e ≈ 36.8$

🚀 Slotted ALOHA doubles efficiency compared to Pure ALOHA.
## 🥊 Pure ALOHA vs Slotted ALOHA — exam table

| Feature                  | Pure ALOHA         | Slotted ALOHA      |
| ------------------------ | ------------------ | ------------------ |
| Transmission timing      | Anytime            | Start of slot only |
| Vulnerable period        | 2T                 | T                  |
| Required synchronization | No                 | Yes                |
| Efficiency               | ~18%               | ~36%               |
| Collisions               | Higher probability | Lower probability  |

