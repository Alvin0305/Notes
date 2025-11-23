Perfect 👍 You’ve installed Wireshark and opened the GUI — now let’s go **step by step** according to your assignment instructions.

---

## 🧭 OVERVIEW

Your goal is to:

* Capture real network traffic while connected to **NITC LAN/Wi-Fi** (no internet).
* Save the capture to a `.pcap` file.
* Analyze it using filters (ARP, DNS, TCP, etc.).

---

## 🪄 STEP 1: Choose the Correct Network Interface

When Wireshark opens, you see a list like:

```
Cisco remote capture: ciscodump
DisplayPort AUX channel monitor: dpauxmon
systemd Journal Export: sdjournal
SSH remote capture: sshdump
UDP Listener remote capture: udpdump
Wi-Fi remote capture: wifidump
```

Those are **remote capture interfaces**, not your actual network ones.

👉 To see your **real network interfaces**:

1. Close that popup and go to the **main Wireshark window**.
2. You’ll see a list under “**Capture**” — something like:

   * `eth0` or `enp3s0` → **for LAN (Ethernet)**
   * `wlan0` or `wlp2s0` → **for Wi-Fi**

If you’re on Wi-Fi at NITC, select **`wlan0` or wlp...`**  
If you’re using a LAN cable, choose **`eth0` or enp...`**.

✅ **Do NOT** select those “remote capture” ones (like ciscodump, sshdump, etc.).
They’re for special setups — not for local traffic.

---

## 🧩 STEP 2: Start Capturing Packets

1. Select your correct interface (e.g., `wlan0`).
2. Click the **blue shark fin icon** 🦈 (top-left) to start capture.
3. You’ll see packets appearing in real time.

---

## ⏱️ STEP 3: Perform Capture as per Assignment

1. Stay connected to the **NITC Wi-Fi or LAN**.
2. Do **not** open any external websites initially.
3. Capture for **10–12 seconds** (this captures background traffic).
4. Now open any browser (e.g., Firefox or Chrome) — open a local webpage or just let it idle.
5. Continue capturing for **15–20 seconds** more.
6. Stop capture using the **red square** ⏹️ button.

---

## 💾 STEP 4: Save the Capture

Go to:

```
File → Export Specified Packets → Save as “nitc_capture.pcap”
```

Save it to your home folder or desktop.

---

## 🧠 STEP 5: Analyze the Capture

Now you’ll answer the assignment questions using this `.pcap` file:

### **Q1: Distinct Protocols**

* Go to **Statistics → Protocol Hierarchy**.
* You’ll get a list of all protocols seen (e.g., ARP, DNS, TCP, UDP, ICMP…).
* Note which protocol has the **highest and lowest** percentage.

---

### **Q2: ARP Analysis**

* Use this filter:

  ```
  arp
  ```
* Click on a **request** packet and note:

  * Sender IP and MAC
  * Target IP and MAC
* Click on a **reply** packet and note differences.

  * Request: asks “Who has IP X?”
  * Reply: responds “IP X is at MAC Y.”

---

### **Q3: DNS Analysis**

* Filter:

  ```
  dns
  ```
* Look for:

  * **Queries (system or user)** → usually port 53 UDP/TCP
  * **Replies** → “Standard query response”
* Determine which ones are automatic (system background lookups) vs. triggered (when you opened browser).

---

### **Q4: TCP Handshake**

* Filter:

  ```
  tcp
  ```
* Find packets with **SYN**, **SYN-ACK**, and **ACK** flags.
* Each sequence of 3 is a handshake.
* Expand the “Transmission Control Protocol” section to see:

  * Sequence number
  * Acknowledgment number

---

### **Q5: Interpretation**

Explain:

* System-driven (e.g., ARP, DNS, ICMP)
* User-driven (e.g., HTTP/HTTPS requests when browser opened)
* Note how many different protocol types occur (shows complexity of LAN).

---

## 🧾 Optional Tip: Color Coding

Wireshark color-codes packets:

* Light blue = TCP
* Light green = UDP
* Yellow = ARP
* Black = errors
  Helps visually identify types easily.

---

Would you like me to show **how to identify user-triggered vs system-triggered DNS requests** in more detail (for Q3)? That’s the trickiest part for most students.
