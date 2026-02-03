## CSMA (Carrier Sense Multiple Access)

ALOHA allowed devices to transmit blindly.  
CSMA improves by **listening before transmitting**.

| Type                           | Behavior                                                               |
| ------------------------------ | ---------------------------------------------------------------------- |
| **1-persistent**               | If channel is busy → keep listening → transmit immediately when free   |
| **Non-persistent**             | If channel is busy → wait random time → retry                          |
| **p-persistent (slotted LAN)** | If channel free → transmit with probability **p**, else wait next slot |
⚠️ Even CSMA can have collisions:
- Because two nodes may sense **idle at the same time** and transmit at once.
- Or one node begins transmitting while the signal hasn't yet reached another node (propagation delay).
## ⚡ CSMA/CD (Carrier Sense Multiple Access with Collision Detection)

Used in **Wired Ethernet (IEEE 802.3)**.
### 🔧 Key idea
Devices **listen while transmitting** to detect collisions.
Steps:
1. Sense channel → if idle, transmit
2. While transmitting, monitor signal
3. If collision detected → **stop immediately**
4. Send **jam signal**
5. Wait **random time** (Binary Exponential Backoff)
6. Retransmit

📌 Collision detection is possible because sender can **compare transmitted signal vs signal on channel**  
(Works only in **wired** networks — impossible in Wi-Fi).
## ⏳ Contention period & minimum frame size (important)

In Ethernet:
- A sender must be able to detect a collision **before it finishes sending the frame**   
- To ensure this, Ethernet defines a **minimum frame size of 64 bytes**
Reason:

$\text{Transmission time} \ge 2 × \text{propagation delay}$

so that a collision anywhere on the wire can be detected before transmission ends.
## 💥 Efficiency summary (exam punch-lines)

| Protocol      | Maximum Efficiency                                            |
| ------------- | ------------------------------------------------------------- |
| Pure ALOHA    | **18.4%**                                                     |
| Slotted ALOHA | **36.8%**                                                     |
| CSMA          | **~60–80%** (approx, improves with reduced propagation delay) |
| CSMA/CD       | **~90% in light load**                                        |
# 🔥 Why do we need a minimum frame size in CSMA/CD?

In CSMA/CD, the sender must be able to **detect a collision WHILE it is still transmitting**.

If the sender finishes transmitting **before** the collision reaches it, then:
- it will **think the frame was delivered successfully**    
- even though it actually collided and got corrupted in the network

So the sender must transmit **long enough** to make sure that:
- if a collision happens **anywhere in the network**, the collision signal has enough time to return back to the sender **before transmission ends**
    
This is the core reason for minimum frame size.

---
# 🚧 The “Worst-Case Collision Scenario”

Consider the **two farthest stations** in the Ethernet network:

`A ------------------------ B (distance = maximum cable length)`

Worst case:
- A starts transmitting
- Just **before** A’s signal reaches B, B starts transmitting
- COLLISION happens at B
- But A won’t know until the collision signal **propagates back** from B to A

So A must still be transmitting when the collision returns.

---
# 🧠 The Key Condition (What your teacher mentioned)

For collision detection to work:
`Transmission time ≥ 2 × Propagation delay`
Or more commonly written as:
$T_{transmit} ≥ 2 × τ$
where:

- $T_{transmit}$ = time needed to send a full Ethernet frame
- τ = **one-way propagation delay** across the maximum cable length

Sometimes class notes use **ε (epsilon)** for frame transmission time:
`ε ≥ 2τ`

💡 Meaning:

> A frame must be long enough so that the station is still transmitting when the worst-case collision comes back.

---
# 📌 How this leads to _minimum frame size_

Transmission time depends on **two things**:

`Frame size (bits) / Data rate (bps)`

So we set:

`Frame_size / Data_rate ≥ 2 × Propagation_delay`

Solving this gives **minimum frame size**.

---

# 🔢 Ethernet example (how 64 bytes come)

- 10 Mbps Ethernet (old classic)
- Maximum cable length allowed → **500 m × 2 repeaters ≈ 2500 m** electrical path
- Signal propagation ≈ **2 × 2.5 µs = 5 µs** round-trip

Recall:
$T_{transmit} ≥ 2 × τ$

To ensure detection:
`Frame_size / 10 Mbps ≥ 50 µs Frame_size ≥ 10,000 bits = 1250 bytes`

But CSMA/CD got optimized using:

- thinner coax
- shorter collision domains
- repeaters that shorten timing

Final requirement standardized to:
`Minimum Ethernet frame size = 64 bytes = 512 bits`
So:  
🎯 Ethernet frame **must be ≥ 64 bytes**  
or else collision detection cannot be guaranteed.

If a device sends less than 64 bytes, the Ethernet NIC **pads** it automatically to reach 64 bytes.

---
# 🧨 What is the contention (collision) period?

The time during which **collisions are possible** after the first bit is transmitted:
`Contention period = 2 × propagation delay = 2τ`

During this time, the sender listens for collision.  
If it **survives longer than the contention period**, the sender knows:  
➡️ No collision occurred  
➡️ Frame is successfully transmitted

---
# 🧩 Memory Hook

| Term        | Meaning                                             |
| ----------- | --------------------------------------------------- |
| τ (tau)     | One-way propagation delay                           |
| 2τ          | Round trip propagation delay — **collision window** |
| ε (epsilon) | Transmission time of the frame                      |
| Condition   | **ε ≥ 2τ → detect collisions safely**               |

---
# 🌟 Short exam-style answer

> In CSMA/CD the sender must detect collisions while transmitting. Therefore, the transmission time of a frame must be greater than or equal to twice the maximum propagation delay across the Ethernet network (ε ≥ 2τ). This ensures that even if a collision occurs at the farthest station, the collision notification returns before transmission ends. To satisfy this condition, Ethernet mandates a minimum frame size of 64 bytes (512 bits). If frames were smaller, a sender could finish transmitting before detecting a collision.

---
## ✔️ CSMA/CD collision period summary (one line)

`Contention window = 2τ = time during which collisions may occur. Minimum frame size ensures the sender transmits throughout this window.`

