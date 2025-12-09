# CCNA – Ethernet Switching (Module 7)

Quick-reference notes for NetAcad CCNA – **Module 7: Ethernet Switching**.  
Focus is on how Ethernet works in a **switched LAN**: frames, MAC addresses,
MAC address tables, and switch forwarding behaviours.

I use this repo to revise:

- How **Ethernet frames** relate to the data link (LLC & MAC) sublayers.
- How **MAC addresses** are structured, written in hex, and used in unicast,
  broadcast, and multicast traffic.
- How switches **learn MAC addresses**, build their MAC address tables, and
  decide whether to forward, flood, or filter frames.
- How different **switching methods and port settings** (store-and-forward,
  cut-through, buffering, duplex, speed, auto-MDIX) impact performance.

---

## Table of Contents

- [Module 7 Overview](#module-7-overview)
- [Module Map](#module-map)

- [7.0 Introduction](#70-introduction)  
  - [7.0.1 Why should I take this module?](#701-why-should-i-take-this-module)  
  - [7.0.2 What will I learn to do in this module?](#702-what-will-i-learn-to-do-in-this-module)

- [7.1 Ethernet Frames](#71-ethernet-frames)  
  - [7.1.1 Ethernet Encapsulation](#711-ethernet-encapsulation)  
  - [7.1.2 Data Link Sublayers](#712-data-link-sublayers)  
  - [7.1.3 MAC Sublayer](#713-mac-sublayer)  
  - [7.1.4 Ethernet Frame Fields](#714-ethernet-frame-fields)  
  - [7.1.5 Check Your Understanding – Ethernet Switching](#715-check-your-understanding--ethernet-switching)  
  - [7.1.6 Lab – Use Wireshark to Examine Ethernet Frames](#716-lab--use-wireshark-to-examine-ethernet-frames)

- [7.2 Ethernet MAC Address](#72-ethernet-mac-address)  
  - [7.2.1 MAC Address and Hexadecimal](#721-mac-address-and-hexadecimal)  
  - [7.2.2 Ethernet MAC Address](#722-ethernet-mac-address)  
  - [7.2.3 Frame Processing](#723-frame-processing)  
  - [7.2.4 Unicast MAC Address](#724-unicast-mac-address)  
  - [7.2.5 Broadcast MAC Address](#725-broadcast-mac-address)  
  - [7.2.6 Multicast MAC Address](#726-multicast-mac-address)  
  - [7.2.7 Lab – View Network Device MAC Addresses](#727-lab--view-network-device-mac-addresses)

- [7.3 The MAC Address Table](#73-the-mac-address-table)  
  - [7.3.1 Switch Fundamentals](#731-switch-fundamentals)  
  - [7.3.2 Switch Learning and Forwarding](#732-switch-learning-and-forwarding)  
  - [7.3.3 Filtering Frames](#733-filtering-frames)  
  - [7.3.4 Video – MAC Address Tables on Connected Switches](#734-video--mac-address-tables-on-connected-switches)  
  - [7.3.5 Video – Sending the Frame to the Default Gateway](#735-video--sending-the-frame-to-the-default-gateway)  
  - [7.3.6 Activity – Switch It!](#736-activity--switch-it)  
  - [7.3.7 Lab – View the Switch MAC Address Table](#737-lab--view-the-switch-mac-address-table)

- [7.4 Switch Speeds and Forwarding Methods](#74-switch-speeds-and-forwarding-methods)  
  - [7.4.1 Frame Forwarding Methods on Cisco Switches](#741-frame-forwarding-methods-on-cisco-switches)  
  - [7.4.2 Cut-Through Switching](#742-cut-through-switching)  
  - [7.4.3 Memory Buffering on Switches](#743-memory-buffering-on-switches)  
  - [7.4.4 Duplex and Speed Settings](#744-duplex-and-speed-settings)  
  - [7.4.5 Auto-MDIX](#745-auto-mdix)  
  - [7.4.6 Check Your Understanding – Switch Speeds and Forwarding Methods](#746-check-your-understanding--switch-speeds-and-forwarding-methods)

- [7.5 Module Practice and Quiz](#75-module-practice-and-quiz)  
  - [7.5.1 What did I learn in this module?](#751-what-did-i-learn-in-this-module)  
  - [7.5.2 Module Quiz – Ethernet Switching](#752-module-quiz--ethernet-switching)

---

## Module 7 Overview

### Goal of the module

Modern LANs are almost always built on **Ethernet switches**.  
To really understand how a LAN behaves, you need to know:

- How Ethernet frames are constructed at Layer 2 and how they relate to the LLC
  and MAC sublayers.
- How **unique MAC addresses** identify interfaces and how switches use them to
  forward traffic.
- How switches **learn**, **store**, and **age out** MAC address entries.
- How different forwarding methods and port settings affect **latency,
  reliability, and throughput**.

This module walks through those ideas step by step so that by the end you can:

- Read and interpret **Ethernet frames** (e.g. in Wireshark).
- Predict how a switch will handle **unicast, broadcast, and multicast** frames.
- Understand what happens inside the **MAC address table** when devices talk.
- Choose appropriate **switching methods, speed/duplex settings, and auto-MDIX**
  behaviour for a given network design.

---

## 7.1 Ethernet Frames

### 7.1.1 Ethernet Encapsulation

- Ethernet is one of the two main LAN technologies used today (the other is **WLAN / IEEE 802.11**).
- Uses **wired media**: UTP, fiber-optic links, and coaxial cables.
- Operates at **OSI Layer 2 (Data Link)** and **Layer 1 (Physical)**.
- It’s a *family* of technologies defined in **IEEE 802.2 and 802.3** standards.
- Typical supported data rates:
  - 10 Mbps  
  - 100 Mbps  
  - 1000 Mbps (1 Gbps)  
  - 10,000 Mbps (10 Gbps)  
  - 40,000 Mbps (40 Gbps)  
  - 100,000 Mbps (100 Gbps)

**Ethernet and the OSI model**

- Upper layers (Application → Transport → Network) are independent of the medium.
- At the **Data Link** layer, Ethernet uses:
  - **LLC** and **MAC** sublayers.
- At the **Physical** layer, Ethernet defines:
  - Signaling, media, and connector standards (twisted pair, fiber, etc.).
- IEEE:
  - **802.2** → LLC
  - **802.3** → MAC + Physical (Ethernet)

---

### 7.1.2 Data Link Sublayers

IEEE 802 LAN/MAN protocols (including Ethernet) split Layer 2 into two sublayers:

- **LLC Sublayer (IEEE 802.2)**
  - Sits between the network software (Layer 3 and above) and the hardware.
  - Inserts information in the frame that **identifies which Network layer protocol** is being carried (IPv4, IPv6, ARP, etc.).
  - Allows multiple Layer-3 protocols to share the same interface and medium.

- **MAC Sublayer (IEEE 802.3, 802.11, 802.15, …)**
  - Implemented mostly in **hardware**.
  - Responsible for **data encapsulation** and **media access control**.
  - Provides **data link addressing (MAC addresses)**.
  - Is tightly integrated with specific physical technologies (Ethernet over copper/fiber, Wi-Fi, Bluetooth/WPAN, …).

Think of it like this:

- LLC = *“What L3 protocol is inside this frame?”*  
- MAC = *“How do I put this on this specific wire or radio and who (which MAC) is it for?”*

---

### 7.1.3 MAC Sublayer

The MAC sublayer does two big jobs: **encapsulation** and **accessing the medium**.

**Data Encapsulation (IEEE 802.3)**

- Defines the **Ethernet frame structure**.
- Handles **Ethernet addressing**:
  - Adds **source MAC** and **destination MAC** so frames can travel from one Ethernet NIC to another on the same LAN.
- Provides **error detection**:
  - Adds a **Frame Check Sequence (FCS)** trailer to detect errors in transit.

**Accessing the Media**

- MAC includes standards for using different media (copper, fiber, etc.) at different speeds.
- Legacy Ethernet with bus or hub:
  - Shared, **half-duplex**.
  - Uses **CSMA/CD** (Carrier Sense Multiple Access / Collision Detection) to avoid/correct collisions.
- Modern switched Ethernet:
  - Uses switches that operate in **full-duplex**.
  - Each link is its own collision-free segment → **no CSMA/CD needed**.

---

### 7.1.4 Ethernet Frame Fields

**Size limits**

- **Minimum** Ethernet frame size: **64 bytes**  
- **Maximum** (expected) frame size: **1518 bytes**
  - Size is from **Destination MAC** through **FCS** (preamble/SFD are not counted).
- Frames \< 64 bytes → **collision fragment / runt frame** → discarded.  
- Frames \> 1500 bytes of data → **jumbo / baby giant frames** (usually special-case support).

**Ethernet Frame Structure (standard 64–1518 bytes)**

From left to right:

1. **Preamble + SFD** (8 bytes total – not counted in size)  
2. **Destination MAC Address** (6 bytes)  
3. **Source MAC Address** (6 bytes)  
4. **Type/Length** (2 bytes)  
5. **Data** (46–1500 bytes) – payload  
6. **FCS** (4 bytes)

**Field details**

- **Preamble + Start Frame Delimiter (SFD)**
  - First 8 bytes on the wire.
  - Used for **synchronization** and to alert receivers that a new frame is starting.

- **Destination MAC Address**
  - 6-byte identifier of the **intended recipient**.
  - Compared with the NIC’s MAC:
    - If it matches (or is a broadcast/multicast address), the NIC accepts the frame.

- **Source MAC Address**
  - 6-byte identifier of the **originating NIC/interface**.

- **Type/Length**
  - 2-byte field that identifies **which upper-layer protocol** is encapsulated.
  - Examples (hex):
    - `0x0800` = IPv4  
    - `0x86DD` = IPv6  
    - `0x0806` = ARP
  - May also be interpreted as a length field depending on the standard.

- **Data**
  - 46–1500 bytes of payload.
  - Carries the Layer-3 PDU (usually an IPv4/IPv6 packet).
  - If the upper-layer data is too short, **padding** is added to reach at least 46 bytes.

- **Frame Check Sequence (FCS)**
  - 4-byte field used for **error detection**.
  - Contains a **CRC (Cyclic Redundancy Check)** value calculated by the sender.
  - Receiver recalculates the CRC; if values differ, the frame is considered corrupted and is dropped.

---

### 7.1.5 Check Your Understanding – Ethernet Switching

#### Question 1  
**Which part of an Ethernet Frame uses a pad to increase the frame field to at least 64 bytes?**

- **Correct answer:** Data field  
- **Why:** Ethernet requires a **minimum frame size of 64 bytes**. If the payload is too small, padding bytes are added **inside the data field** so the frame reaches this minimum size.

---

#### Question 2  
**Which part of an Ethernet frame detects errors in the frame?**

- **Correct answer:** Frame Check Sequence  
- **Why:** The **FCS** field contains a CRC value calculated by the sender. The receiver recalculates the CRC and compares it with the FCS; a mismatch means the frame is corrupted and must be discarded.

---

#### Question 3  
**Which part of an Ethernet Frame describes the higher-layer protocol that is encapsulated?**

- **Correct answer:** EtherType  
- **Why:** The **Type/Length (EtherType)** field identifies **which Layer 3 protocol** is carried inside the frame (for example IPv4, IPv6, ARP), so the NIC knows where to send the payload.

---

#### Question 4  
**Which part of an Ethernet Frame notifies the receiver to get ready for a new frame?**

- **Correct answer:** Preamble  
- **Why:** The **Preamble** is a sequence of bits sent before the frame. It lets the receiver **synchronize** and “wake up,” getting ready to receive the rest of the frame.

---

#### Question 5  
**Which data link sublayer controls the network interface through software drivers?**

- **Correct answer:** LLC  
- **Why:** The **Logical Link Control (LLC) sublayer** (IEEE 802.2) is the part of Layer 2 that works in **software**. It interfaces between the network-layer protocols and the NIC via **drivers**, telling the MAC/hardware how to handle frames for each protocol.

---

#### Question 6  
**Which data link sublayer works with the upper layers to add application information for delivery of data to higher level protocols?**

- **Correct answer:** LLC  
- **Why:** LLC adds **protocol identification and control information** so that multiple Layer 3 protocols (IPv4, IPv6, etc.) can share the same NIC and media. It is the “glue” between upper layers and the MAC sublayer.

---

#### Question 7  
**What is a function of the MAC sublayer? (Choose three.)**

Options:  
1. Controls access to the media  
2. Communicates between software at the upper layers and the device hardware at the lower layers  
3. Uses CSMA/CD or CSMA/CA to support Ethernet technology  
4. Checks for errors in received bits  
5. Allows multiple Layer 3 protocols to use the same network interface and media  

- **Correct answers:**  
  - ✅ Controls access to the media (1)  
  - ✅ Uses CSMA/CD or CSMA/CA to support Ethernet technology (3)  
  - ✅ Checks for errors in received bits (4)

- **Why:**  
  - The **MAC sublayer** is responsible for **media access control** – deciding who can send and when.  
  - It uses **CSMA/CD (wired)** and **CSMA/CA (wireless)** algorithms to share the medium.  
  - It also performs **error detection** using the FCS at the end of the frame.  

  Options (2) and (5) are **LLC** responsibilities, not MAC.

---

 # CCNA – Lab 7.1.6: Use Wireshark to Examine Ethernet Frames

> NetAcad CCNA, Module 7 – Ethernet Switching  
> Lab file: **7.1.6 Lab – Use Wireshark to Examine Ethernet Frames**

---

## Lab Goals

- Review the structure of an **Ethernet II** frame and the purpose of each header field.
- Use **Wireshark** to inspect captured Ethernet frames.
- See how ARP and ICMP traffic looks at Layer 2 and Layer 3.
- Relate MAC addresses, OUIs, IP addresses and frame types.

---

## Topology & Example Addressing

Single Windows PC connected to a default‑gateway router on a LAN.

Example data from the official lab PDF:

- PC IPv4 address: `192.168.1.147`
- Default gateway IPv4: `192.168.1.1`
- PC NIC MAC address: `f0:1f:af:50:fd:c8` (Dell NIC, unicast)
- Broadcast MAC address: `ff:ff:ff:ff:ff:ff`

Your own capture will use whatever addressing exists on your machine.

---

## Part 1 – Examine Header Fields in an Ethernet II Frame

### Step 1 – Ethernet II header recap

Standard Ethernet II frame fields and sizes:

| Field              | Size                     |
|--------------------|--------------------------|
| Preamble           | 8 bytes (7B preamble + 1B SFD) |
| Destination address| 6 bytes                  |
| Source address     | 6 bytes                  |
| Type               | 2 bytes                  |
| Data (payload)     | 46–1500 bytes            |
| FCS                | 4 bytes                  |

### Step 2 – PC network configuration

On Windows:

```powershell
ipconfig /all
```

From the lab example:

- PC IPv4 address: `192.168.1.147`
- Default gateway IPv4: `192.168.1.1`
- PC physical (MAC) address: `f0-1f-af-50-fd-c8`

These values are used again when reading the Wireshark capture.

### Step 3 – ARP + ICMP capture overview

Wireshark capture shows:

1. **ARP request** – PC asks “Who has 192.168.1.1? Tell 192.168.1.147”.
2. **ARP reply** – Default gateway replies with its MAC address.
3. Four ICMP **Echo Request** (ping) frames from the PC to the gateway.
4. Four ICMP **Echo Reply** frames back from the gateway.

A display filter is applied so that only **ARP** and **ICMP** frames are visible:

```text
arp || icmp
```

(Or `arp or icmp`.)

### Step 4 – Ethernet II header contents of an ARP request

Ethernet II header for the first ARP request:

| Field                 | Example Value                              | Notes |
|-----------------------|--------------------------------------------|------|
| Preamble              | *Not shown in capture*                     | Synchronization bits processed by NIC hardware. |
| Destination MAC       | `ff:ff:ff:ff:ff:ff`                        | Broadcast to all devices on the LAN. |
| Source MAC            | `f0:1f:af:50:fd:c8`                        | PC NIC (Dell_50:fd:c8). |
| Type                  | `0x0806`                                   | EtherType = ARP. |
| Data                  | ARP payload                                | Contains sender/target MAC + IP addresses. |
| FCS                   | *Not shown in capture*                     | Frame Check Sequence used for error detection. |

**Q1 – What is significant about the destination address field?**  
- It is the **broadcast MAC address** `ff:ff:ff:ff:ff:ff`, so every device on the LAN receives the ARP request.

**Q2 – Why does the PC send a broadcast ARP before the first ping?**  
- The PC knows the default gateway’s **IP address** (`192.168.1.1`) but not its **MAC address**.  
- ARP is used to map IP → MAC, so the PC broadcasts “Who has 192.168.1.1?” and learns the router’s MAC before it can send the unicast ICMP ping frame.

**Q3 – What is the MAC address of the source in the first frame?**  
- From the example capture: **`f0:1f:af:50:fd:c8`** (the PC’s NIC).

### ARP Reply – OUI and serial

The ARP reply frame shows the source MAC address belonging to the device that answered (often the router).

For the example PC NIC:

- **MAC:** `f0:1f:af:50:fd:c8`
- **OUI (Organizationally Unique Identifier):** first 3 bytes → `f0:1f:af` → identifies the **vendor** (Dell in this case).
- **NIC serial number:** last 3 bytes → `50:fd:c8` → unique host portion assigned by the vendor.

**Q4 – What is the Vendor ID (OUI) of the source NIC in the ARP reply?**  
- In the sample output: **`f0:1f:af`** (Dell).  
  On your own PC, take the first three octets of the source MAC.

**Q5 – What portion of the MAC address is the OUI?**  
- The **first 24 bits** / **first 3 bytes** / **first 6 hex digits**.

**Q6 – What is the NIC serial number of the source?**  
- The **last 24 bits** / **last 3 bytes** of the MAC.  
- Example with `f0:1f:af:50:fd:c8`: the NIC serial portion is **`50:fd:c8`**.

---

## Part 2 – Use Wireshark to Capture and Analyze Ethernet Frames

### Step 1 – Find the default gateway

In a Windows command prompt:

```powershell
ipconfig
```

Look under your active adapter for **Default Gateway**.

- Example from the lab: **`192.168.1.1`**

### Step 2 – Start capture on the NIC

1. Open **Wireshark**.
2. Select your active Ethernet (or Wi‑Fi) interface.
3. Click **Start Capturing Packets**.

You should see a continuous list of frames (ARP, IPv6, IPv4, TCP, etc.).

### Step 3 – Filter for ICMP

Apply this display filter:

```text
icmp
```

The filter does not change what is captured, only what is displayed.

### Step 4 – Ping the default gateway

In a command prompt:

```powershell
ping 192.168.1.1
```

(Replace with your own default gateway if different.)

You should see four Echo Request / Echo Reply messages complete successfully.

### Step 5 – Stop the capture

Click **Stop Capturing Packets** in Wireshark.

Now only the ICMP frames from your ping should be visible (because of the filter).

### Step 6 – Examine the first Echo Request frame

Select the first frame with **“Echo (ping) request”** in the **Info** column.

#### 6a – Frame length

The first line in the middle *Packet Details* pane shows the total **frame length in bytes** (e.g. 98 bytes). This is the entire Ethernet frame on the wire.

#### 6b – Ethernet II header

The second line in Packet Details is **“Ethernet II”** and contains:

- Destination MAC
- Source MAC
- EtherType (e.g. `0x0800` for IPv4)

**Q7 – What is the MAC address of the PC NIC?**  
- Example from the lab: **`f0:1f:af:50:fd:c8`**  
  (On your own machine, this should match your adapter’s physical address from `ipconfig /all`.)

**Q8 – What is the default gateway’s MAC address?**  
- This is the **destination MAC** in the Echo Request – the MAC of the router’s LAN interface.  
- The exact value depends on your router, e.g. something like `a4:b2:39:1c:d7:10`.

Expand the **Ethernet II** line (click the ▸ icon) to see these addresses clearly.

**Q9 – What type of frame is displayed?**  
- The second line explicitly says: **Ethernet II**.  
- The EtherType field is `0x0800`, indicating that the payload is IPv4.

#### 6c – IP header and ICMP data

Expand the **Internet Protocol Version 4** line.

Here you will see:

- **Source IP address** – your PC.
- **Destination IP address** – the default gateway.

From the example:

- Source IP: `192.168.1.147`
- Destination IP: `192.168.1.1`

**Q10 – What is the source IP address?**  
- Example: **`192.168.1.147`**

**Q11 – What is the destination IP address?**  
- Example: **`192.168.1.1`**

Expand the **Internet Control Message Protocol** line to see the ICMP portion, including the payload.

In Windows pings, the data part contains ASCII characters:

```text
abcdefghijklmnopqrstuvwabcdefghi
```

The last two highlighted octets correspond to the last two characters of that string.

**Q12 – What do the last two highlighted octets spell?**  
- They spell **“hi”**.

#### 6d – Echo Reply

Move to the next ICMP frame, which should be **Echo (ping) reply** from the gateway.

Notice:

- The **source / destination MAC addresses are reversed** compared to the request.
- The **source / destination IP addresses are also reversed**.

**Q13 – What device and MAC address is displayed as the destination address?**  
- It is the **PC**, with MAC address **`f0:1f:af:50:fd:c8`** in the example.  
- In general: destination = MAC of the host that sent the original Echo Request.

### Step 7 – Capture packets to a remote host

Repeat the procedure, but this time ping a remote host such as **www.cisco.com**:

1. Start a new capture in Wireshark (discard old packets if prompted).
2. Keep the `icmp` display filter.
3. From a command prompt:

   ```powershell
   ping www.cisco.com
   ```

4. Stop the capture.

#### 7a – First Echo Request to remote host

Select the first Echo Request to the remote server.

**Q14 – In the first Echo Request frame, what are the source and destination MAC addresses?**

- **Source MAC:** your PC NIC.
- **Destination MAC:** the MAC address of your **default gateway** (same as in the earlier local ping).

Even though the IP destination is on the internet, at Layer 2 the frame is only delivered to the **next hop router**.

**Q15 – What are the source and destination IP addresses in the data field?**

- **Source IP:** your PC’s IPv4 address.
- **Destination IP:** the IPv4 address of `www.cisco.com` resolved by DNS.

These two fields are different from the MAC addresses – they describe end‑to‑end logical addressing, not just the local hop.

**Q16 – Why did the destination IP address change, but the destination MAC address stayed the same?**

- The ping to `www.cisco.com` is destined for a **remote IP host**, so the **destination IP** is the server’s address instead of the default gateway.
- However, on the **local Ethernet segment** your PC can only reach the **default gateway router** directly.  
  So it sends the frame **to the router’s MAC address**; the router then forwards the packet to the next hop.  
- Result: **Same destination MAC (gateway), new destination IP (remote server).**

---

## Reflection

**Q17 – Wireshark does not show the preamble. What does the preamble contain?**

- The preamble consists of **7 bytes of alternating 1s and 0s**, followed by a **1‑byte Start Frame Delimiter (SFD)**.  
- Together, these 8 bytes allow receiving NICs to **synchronize their clocks** and recognize the **start of a new Ethernet frame**.  
- Because the preamble is handled entirely by hardware and never passed up the stack, Wireshark (which sees frames above the physical layer) does **not** display it.

---

## Key Takeaways

- Ethernet II frames wrap Layer 3 packets with **MAC addresses, EtherType, and FCS**.
- **ARP** uses broadcast frames to discover MAC addresses for known IP addresses.
- ICMP echo traffic makes it easy to see how **source/destination MAC and IP addresses** behave for both local and remote destinations.
- **OUIs** identify the vendor (first 3 bytes of MAC), while the last 3 bytes are the NIC’s unique serial portion.
- The **default gateway’s MAC** is the destination for any traffic leaving the local subnet, even when the final IP destination is on the internet.


