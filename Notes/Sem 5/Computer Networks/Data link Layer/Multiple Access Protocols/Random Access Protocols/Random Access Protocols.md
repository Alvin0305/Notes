## 🚦Why do we need MAC Protocols?

Multiple devices share the **same transmission medium** (wireless or wired LAN).  
If two devices transmit at the same time → **collision** → packets get corrupted.

MAC protocols define **rules for how and when devices may transmit** so that collisions are minimized.

## 🧠 Exam-ready paragraph

> ALOHA protocols allow random access to a shared medium. Pure ALOHA transmits immediately and has a vulnerable period of 2T, resulting in a maximum efficiency of 18.4%. Slotted ALOHA restricts transmissions to time slots, reducing the vulnerable period to T and doubling efficiency to 36.8%. CSMA improves efficiency by sensing the channel before transmission, but collisions still occur due to propagation delay and simultaneous sensing. CSMA/CD, used in Ethernet, senses the medium during transmission and aborts on collision, followed by binary exponential backoff, resulting in much higher efficiency.

![[ALOHA]]

![[CSMA]]
