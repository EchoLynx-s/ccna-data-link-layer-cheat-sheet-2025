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

---

## 7.2 Ethernet MAC Address

In this topic I focus on how Ethernet uses **MAC addresses** at Layer 2 and why
they are written in **hexadecimal**.

---

### 7.2.1 MAC Address and Hexadecimal

In networking we use different number systems:

- **IPv4** → usually written in **decimal** (base 10) and shown in **binary** (base 2).
- **IPv6 and Ethernet MAC addresses** → written in **hexadecimal** (base 16).

Hexadecimal:

- Digits: **0–9** and **A–F** (A=10, B=11, …, F=15).
- **1 hex digit = 4 bits**, so:
  - 0000₂ = 0₁₆, 0001₂ = 1₁₆, …, 1111₂ = F₁₆.
- Because 1 byte = 8 bits = 2 hex digits, binary `00000000` to `11111111`
  maps to hex **00 to FF**.

Hex values are often written as:

- `0x73` (prefix `0x`), or  
- `73₁₆`, or  
- `73H`.

When converting between decimal, binary, and hex, you can go via binary:
decimal ↔ **binary** ↔ hexadecimal.

---

### 7.2.2 Ethernet MAC Address

On an Ethernet LAN, all devices share the same Layer 2 medium.  
Each **network interface card (NIC)** needs a unique **MAC address** so frames
can identify the physical **source** and **destination**.

Key facts:

- An Ethernet MAC address is a **48-bit** value.
- 48 bits = **6 bytes** = **12 hexadecimal digits**.

Vendors obtain a unique **Organizationally Unique Identifier (OUI)** from the IEEE:

- First **24 bits (3 bytes / 6 hex digits)** → **OUI** (vendor identifier).
- Last **24 bits (3 bytes / 6 hex digits)** → vendor-assigned device value.

Example structure:

- OUI (Cisco example): `00-60-2F`
- Vendor-assigned: `3A-07-BC`
- Full MAC: `00-60-2F-3A-07-BC`

Vendors must make sure each device/interface receives a **unique** MAC.  
Duplicates can still appear due to manufacturing mistakes, VM cloning, or manual
changes. In those cases the MAC can be changed in software or by replacing the
NIC.

---

### 7.2.3 Frame Processing

The MAC address is often called the **burned-in address (BIA)** because it is
stored in **ROM** on the NIC. At boot:

1. The NIC copies its MAC from ROM into RAM.
2. When the device sends an Ethernet frame, the header includes:
   - **Source MAC address** (MAC of the sending NIC)
   - **Destination MAC address** (MAC of the intended receiver)

When a NIC receives an Ethernet frame it:

1. Checks the **destination MAC** against:
   - Its own MAC (unicast),
   - The **broadcast MAC**, or
   - Any **multicast MACs** it is listening for.
2. If there is **no match**, the frame is dropped.
3. If there **is** a match, the frame is passed up the OSI layers and
   de-encapsulated.

Any device that sends or receives Ethernet frames—PCs, servers, printers,
mobile devices, routers—has an Ethernet NIC and therefore at least one **MAC
address**.

---

### 7.2.4 Unicast MAC Address

Ethernet uses different MAC address types for **unicast, broadcast, and
multicast** traffic.

A **unicast MAC address**:

- Identifies **one specific device** on the LAN.
- Is used when one device sends a frame to exactly **one destination**.

Example:

- Host with IPv4 address **192.168.1.5** requests a web page from a server at
  **192.168.1.200**.
- For the packet to be sent, the source needs the **destination MAC**:
  - For IPv4 it uses **ARP (Address Resolution Protocol)**.
  - For IPv6 it uses **Neighbor Discovery (ND)**.
- The resulting frame has:
  - **Source MAC** = unicast MAC of the sender’s NIC.
  - **Destination MAC** = unicast MAC of the server’s NIC.

The **source MAC address is always unicast**.

---

### 7.2.5 Broadcast MAC Address

A **broadcast MAC address** is processed by **every device** on the local
Ethernet LAN (broadcast domain).

Characteristics:

- Destination MAC is **FF-FF-FF-FF-FF-FF** (48 ones in binary).
- The switch **floods** the frame out all ports except the incoming one.
- Routers **do not forward** Ethernet broadcasts to other networks.

If the encapsulated packet is an IPv4 broadcast:

- The destination IPv4 address has all **1s in the host portion**, e.g.
  **192.168.1.255**.
- When that IPv4 packet is placed in an Ethernet frame, the destination MAC is
  **FF-FF-FF-FF-FF-FF**.

Examples:

- **DHCP for IPv4** uses Ethernet broadcasts together with IPv4 broadcast
  addresses.
- **ARP Requests** are also sent as Ethernet broadcasts (even though they carry
  an ARP message, not an IPv4 broadcast packet).

These broadcasts ensure that all devices on the LAN receive and can respond to
important discovery or configuration messages.

---

# 7.2.6 Multicast MAC Address & 7.2.7 Lab – View Network Device MAC Addresses

This markdown is part of my CCNA *Module 7 – Ethernet Switching* notes.  
Focus here is on **multicast MAC addressing** and a hands-on lab where I
inspect **real MAC addresses on a PC and a switch**.

---

## 7.2.6 Multicast MAC Address

### What is an Ethernet multicast frame?

An **Ethernet multicast frame** is delivered to a *group* of devices on the
same Ethernet LAN, instead of a single host (unicast) or all hosts
(broadcast).

Only devices that are members of the specific multicast group will process
the frame; other devices ignore it.

### Key properties of Ethernet multicast

- The frame has a **special destination MAC address** that indicates
  multicast:
  - For **IPv4 multicast traffic**, the destination MAC **starts with**
    `01-00-5E` (the remaining bits come from the IPv4 multicast address).
  - For **IPv6 multicast traffic**, the destination MAC **starts with**
    `33-33`.
- There are additional **reserved multicast MAC addresses** used for
  non-IP protocols, for example:
  - **STP** (Spanning Tree Protocol)
  - **LLDP** (Link Layer Discovery Protocol)
- By default, a switch **floods multicast frames** out all ports in the
  VLAN **except the incoming port**, *unless* features such as
  **multicast/IGMP snooping** are configured to limit this.
- Routers **do not forward multicast frames** by default. They only forward
  them if **multicast routing** is configured.

### Multicast IP addressing

If the encapsulated data is an IP multicast packet, the destination **IP**
address is also multicast:

- IPv4 multicast range: **224.0.0.0 – 239.255.255.255**
- IPv6 multicast range: all addresses starting with **`ff00::/8`**

Important points:

- A **multicast IP address** represents a *group of hosts* (a host group).
- Because of that, it **can only be used as a destination** address.
- The **source** address in any multicast packet is always **unicast**.

### Mapping IP multicast to MAC multicast

To deliver a multicast packet on the local network, the **multicast IP
address is mapped to a multicast MAC address**.  
The host NIC listens for the specific multicast MACs that correspond to the
multicast groups it has joined.

This mapping ensures that only hosts in the multicast group receive and
process the frame, even though the frame is still delivered using normal
Ethernet mechanisms.

### Where is multicast used?

Common examples:

- **Routing protocols** (e.g., sending routing updates to a group of
  routers).
- **Network discovery and control protocols**.
- Some **applications** such as video streaming, imaging, and other
  one-to-many / many-to-many communication patterns  
  (although “real” multicast applications are less common in many
  enterprise networks).

---

## 7.2.7 Lab – View Network Device MAC Addresses

### Lab goal

Use a very small topology (one **switch** and one **PC**) to:

1. Discover the **MAC address of a PC**.
2. See how the **switch learns and stores MAC addresses** in its MAC
   address table.
3. Relate this to **Layer 2 broadcasts** and **Layer 3 broadcasts**.

### Topology & addressing

**Devices**

| Device | Interface / VLAN | IPv4 Address  | Subnet Mask      |
|--------|------------------|--------------|------------------|
| S1     | VLAN 1           | 192.168.1.2  | 255.255.255.0    |
| PC-A   | NIC              | 192.168.1.3  | 255.255.255.0    |

- PC-A is connected to **S1 FastEthernet 0/6 (Fa0/6)**.

---

### Part 1 – View the MAC address of the PC

#### Step 1 – Display the MAC address on PC-A

On PC-A:

1. Open **Command Prompt**.
2. Run:

   ```bash
   ipconfig /all
   ```

3. Find the **Ethernet adapter** used to connect to S1.
4. Note:
   - **IPv4 Address** (should be `192.168.1.3`).
   - **Physical Address** – this is the **MAC address** of PC-A.

> In the original lab you write down this address; in my notes I simply
> refer to it as **MAC(PC-A)**.

#### Step 2 – Verify connectivity between S1 and PC-A

On the **switch S1** (console or SSH):

1. Enter privileged EXEC mode:

   ```text
   S1> enable
   ```

2. From S1, ping PC-A:

   ```text
   S1# ping 192.168.1.3
   ```

3. Successful replies confirm:
   - VLAN 1 interface (`192.168.1.2`) is up.
   - PC-A (`192.168.1.3`) is reachable at Layer 3.
   - The devices have exchanged frames, so **MAC learning** can occur.

---

### Part 2 – View the switch MAC address table

#### Step 1 – Display the complete MAC address table

On S1:

```text
S1# show mac address-table
```

The output lists:

- Several **STATIC** MAC entries used internally by the switch (CPU, STP,
  etc.).
- One **DYNAMIC** entry learned on the access port connected to PC-A, for
  example:

```text
   VLAN    MAC Address       Type       Ports
   ----    -----------       ----       -----
   ...
     1     5c26.0a24.2a60    DYNAMIC    Fa0/6
Total Mac Addresses for this criterion: 21
```

This dynamic entry is **MAC(PC-A)** on **Fa0/6**.

---

### Lab Questions & Answers

#### Question – Did the switch display the MAC address of PC-A?  
If yes, what port was it on?

- **Answer:** Yes. The switch shows PC-A’s MAC address as a **dynamic** entry
  on interface **FastEthernet 0/6 (Fa0/6)**.
- **Why?**
  - When S1 received frames from PC-A on Fa0/6, it **learned** the source
    MAC address and added it to the MAC address table, associating that MAC
    with Fa0/6.
  - Any unicast frames **to** MAC(PC-A) are now forwarded **only** out
    Fa0/6.

---

### Reflection Questions

#### 1. Can you have broadcasts at the Layer 2 level?  
If so, give an example of a typical MAC broadcast address.

- **Answer:** Yes. At Layer 2, Ethernet supports **broadcast frames**.
  The standard MAC broadcast address is:

  ```text
  FF-FF-FF-FF-FF-FF
  ```

- **Why?**
  - This MAC address is all 1s in binary (48 bits of 1).
  - Every Ethernet NIC treats `FF-FF-FF-FF-FF-FF` as **“listen and process
    this frame”**, regardless of its own unicast MAC.
  - Switches flood Layer 2 broadcasts out all ports in the same VLAN (except
    the incoming port).

#### 2. Can you have broadcasts at the Layer 3 level?  
If so, give an example of a typical IP broadcast address.

- **Answer:** Yes. IPv4 supports **broadcast IP addresses**. Common examples:

  - **Limited broadcast:** `255.255.255.255`
  - **Directed broadcast** to a specific subnet, e.g.
    `192.168.1.255` for the subnet `192.168.1.0/24`.

- **Why?**
  - A Layer 3 broadcast address means “deliver to **all hosts on this
    network**”.
  - When an IPv4 broadcast is encapsulated in Ethernet, it is sent to the
    Layer 2 broadcast MAC `FF-FF-FF-FF-FF-FF`, so that every host on the
    LAN sees the packet.

---

### What I’m supposed to understand here

- **Multicast MAC and IP addressing** allow one sender to efficiently reach
  **a group of receivers**, without sending separate unicast copies.
- **Switches learn MAC addresses** dynamically by watching the **source**
  MAC of incoming frames and building a **MAC address table**.
- **Layer 2 broadcasts** use the all-ones MAC address and are flooded across
  a broadcast domain.
- **Layer 3 broadcasts** use special IPv4 addresses (like 255.255.255.255
  or the subnet broadcast) and, when carried over Ethernet, still rely on
  the Layer 2 broadcast MAC address.

---

# CCNA – Ethernet Switching (Module 7)

## 7.3 The MAC Address Table

This section focuses on how a Layer 2 switch learns MAC addresses, builds its **MAC address table** (CAM
table), and uses it to decide whether to **forward, flood, or filter** Ethernet frames.


---

### 7.3.1 Switch Fundamentals

A Layer 2 Ethernet switch forwards frames based only on **Layer 2 MAC addresses**. It does **not** care what
protocol is inside the data field (IPv4, IPv6, ARP, ND, etc.).

* If a switch behaved like a hub and simply repeated every incoming bit out of all ports, the LAN would
  quickly become congested.
* Instead, the switch keeps a **MAC address table** that maps `MAC address → switch port`. This is sometimes
  called a **CAM (Content Addressable Memory) table**, but in this module it is referred to simply as the
  MAC address table.
* Each time the switch receives a frame, it consults this table to decide what to do with the frame.


---

### 7.3.2 Switch Learning and Forwarding

The switch has two core jobs:

1. **Learn** MAC addresses from the source of incoming frames.
2. **Forward** frames toward the correct destination, or flood them if the destination is unknown.

#### Learning – Examine the Source MAC Address

For every frame that arrives on a port, the switch:

1. Looks at the **source MAC address** and the **ingress port**.
2. If that MAC address is **not** already in the table, it adds a new entry:
   `source MAC → incoming port`.
3. If the MAC address **is** already there on the same port, it simply refreshes the **aging timer**
   (typically **5 minutes**).
4. If the MAC address is present but mapped to a **different port**, the switch treats this as a move and
   **updates** the entry to the new port.

**Example – PC‑A sends to PC‑D**

* Four hosts (A–D) are connected to switch ports 1–4.
* PC‑A (MAC `00-0A`) on port 1 sends a frame to PC‑D (MAC `00-0D`).
* The switch sees the frame arrive on **port 1** with **source MAC 00-0A**.
* It creates/updates the entry: `00-0A → port 1`.

At this moment the MAC address table contains **only** PC‑A’s entry.

#### Forwarding – Find the Destination MAC Address

After learning from the source, the switch checks the **destination MAC**:

* If the destination is a **unicast** MAC address and **exists** in the MAC table, the frame is forwarded
  **only out that specific port**.
* If the destination is a unicast MAC address that is **not** in the table, the frame is treated as an
  **unknown unicast** and is **flooded** out **all ports except the incoming one**.
* If the destination is a **broadcast** or **multicast** MAC address, the frame is also **flooded** out all
  ports except the one it came in on.

**Continuing the example**

* The destination MAC for PC‑D (`00-0D`) is **not** yet in the MAC table.
* The switch therefore **floods** the frame out ports 2, 3, and 4 (all ports except port 1).


---

### 7.3.3 Filtering Frames

As more frames traverse the switch, the MAC address table fills up, allowing the switch to **filter** frames
and send them only where they are needed.

#### Step 1 – PC‑D to Switch (Learning PC‑D)

* PC‑D (MAC `00-0D`) on port 4 replies to PC‑A (MAC `00-0A`).
* The frame arrives on **port 4** with **source MAC 00-0D** and **destination MAC 00-0A**.
* The switch adds a new entry: `00-0D → port 4`.

Now the MAC address table has at least:

* `00-0A → port 1`
* `00-0D → port 4`

#### Step 2 – Switch to PC‑A

* Because the switch already has `00-0A → port 1` in the table,
* It forwards the reply **only out port 1** to PC‑A.
* This is an example of **frame filtering** – instead of flooding to all ports, the switch sends the frame
  only where it is needed.

#### Step 3 – PC‑A to Switch to PC‑D (Refresh + Filter)

* PC‑A sends another frame to PC‑D.
* The switch again sees **source MAC 00-0A on port 1** and **refreshes the aging timer** for that entry.
* Because it already knows that `00-0D → port 4`, the switch forwards the frame **only out port 4**.

Result: traffic is now efficiently switched only between ports 1 and 4 for this conversation.


---

### 7.3.4 Video – MAC Address Tables on Connected Switches

When switches are connected to each other, a single port can learn **multiple MAC addresses**. The video
walks through how two switches (S1 and S2) learn addresses when PC‑A and PC‑B communicate.

**Scenario summary**

* PC‑A sends a frame to PC‑B.
  * **Source MAC:** `00-0A` (PC‑A)  
    **Destination MAC:** `00-0B` (PC‑B)
* S1 receives the frame:
  1. Learns `00-0A` on its port connected to PC‑A and adds it to its MAC table.
  2. Does not yet know `00-0B`, so it **floods** the frame out all other ports.
* PC‑B receives the flooded frame:
  * Destination MAC matches its own, so it accepts the frame.
* The frame also reaches S2:
  1. S2 learns `00-0A` on the port facing S1.
  2. S2 still does not know `00-0B`, so it also floods the frame out its other ports.
  3. Hosts whose MAC does **not** match the destination simply **discard** the frame.

Next, PC‑B replies to PC‑A:

* **Source MAC:** `00-0B` (PC‑B)  
  **Destination MAC:** `00-0A` (PC‑A)
* S1 receives the frame from PC‑B:
  1. Learns `00-0B` on the PC‑B-facing port.
  2. Already has `00-0A → PC‑A port` in its table, so it forwards directly **only to that port**.
* PC‑A receives the reply and accepts it because the destination MAC matches its own address.

At the end of this exchange:

* **S1** knows MACs for PC‑A and PC‑B on their respective ports.
* **S2** has learned MACs for the hosts reachable through the inter‑switch link.
* A single inter‑switch port can thus map to **many** MAC addresses in the table.


---

### 7.3.5 Video – Sending the Frame to the Default Gateway

This video shows what happens when PC‑A sends traffic to a **remote network**. In that case, frames go to
the **default gateway’s MAC address**, not directly to the remote host.

**Outbound from PC‑A to the Internet**

* PC‑A needs to reach an IP address on another network.
* It builds an Ethernet frame with:
  * **Source MAC:** PC‑A
  * **Destination MAC:** router (default gateway, e.g. `00-0D`)
* The frame is sent to switch S1.
  1. S1 refreshes the MAC table entry for PC‑A’s MAC (already known).
  2. S1 does not yet know the router’s MAC, so it **floods** the frame out all other ports.
* Other hosts (PC‑B, PC‑C) receive the flooded frame, see that the destination MAC does **not** match
  their own, and drop it.
* S2 also receives and floods the frame until it reaches the router.
* The router’s interface MAC **does** match the destination, so the router accepts the frame and processes
  the IP packet for forwarding to the remote network.

**Return traffic from the router back to PC‑A**

* The returning IP packet has:
  * **Source IP:** remote host  
    **Destination IP:** PC‑A
* At Layer 2, the router sends a frame with:
  * **Source MAC:** router
  * **Destination MAC:** PC‑A
* The frame goes to S2:
  1. S2 doesn’t yet have the router MAC in its table, so it adds it.
  2. S2 already knows PC‑A’s MAC, so it forwards the frame out only the correct port toward S1.
* S1 repeats the process:
  1. Learns the router MAC on its S2‑facing port (if new).
  2. Uses its table entry for PC‑A to send the frame only out PC‑A’s port.
* PC‑A sees its own MAC as the destination and accepts the frame.

Key point: even for remote destinations, the **Ethernet frame** is always addressed **between local MAC
addresses**: host ↔ default‑gateway ↔ next hop, and so on.


---

### 7.3.6 Activity – “Switch It!”

The interactive **Switch It!** activity asks you to decide how a switch forwards a single frame based on:

* The **source MAC** and **destination MAC** in the frame.
* The current **MAC address table** entries for each switch port.

Typical questions you answer:

1. **“Where will the switch forward the frame?”**  
   You choose the correct outgoing interface (Fa1, Fa2, … Fa12) depending on whether:
   * The destination MAC is known (forward only to that port), or
   * The destination MAC is unknown or a broadcast (flood out all ports except the incoming one).

2. **“Which statements are true when the switch forwards the frame?”** Examples include:
   * Whether the switch **adds** the source MAC to the MAC table (when it is new).
   * Whether the frame is **broadcast** to all ports.
   * Whether it is a **unicast** that is forwarded to a **single** port or **flooded**.
   * Whether the frame would ever be **dropped** at the switch.

The goal of the activity is to practice reading the MAC address table and predicting the switch’s behaviour.


---

## 7.3.7 Lab – View the Switch MAC Address Table

This lab uses two switches and two PCs to show how switches learn MAC addresses and how you can
inspect and clear the MAC address table from the CLI. (Lab: *View the Switch MAC Address Table*.) fileciteturn4file0


### Topology and Addressing

Devices and VLAN 1 addressing:

| Device | Interface | IP address     | Subnet mask       |
|--------|-----------|----------------|-------------------|
| S1     | VLAN 1    | 192.168.1.11   | 255.255.255.0     |
| S2     | VLAN 1    | 192.168.1.12   | 255.255.255.0     |
| PC‑A   | NIC       | 192.168.1.1    | 255.255.255.0     |
| PC‑B   | NIC       | 192.168.1.2    | 255.255.255.0     |

S1 connects to PC‑A, S2 connects to PC‑B, and S1–S2 are linked by a FastEthernet trunk (F0/1 in the
topology).


### Part 1 – Build and Configure the Network

1. **Cable the topology**
   * Connect PC‑A to S1 (e.g., F0/6), PC‑B to S2 (e.g., F0/18), and S1 to S2 via F0/1.
   * Use straight‑through Ethernet cables; on Catalyst 2960s the ports are autosensing.

2. **Configure the PCs**
   * On PC‑A and PC‑B, configure the IPv4 addresses and subnet masks from the table above.

3. **Initialize and reload the switches (if needed)**
   * Erase any existing startup configuration and reload to start from a clean state.

4. **Apply basic switch configuration**
   * Give each switch a hostname (`S1`, `S2`).
   * Configure **VLAN 1** with the management IP address from the table.
   * Set console and VTY passwords to `cisco`.
   * Set the privileged EXEC password to `class`.


### Part 2 – Examine the Switch MAC Address Table

The main goal is to see **how the MAC table changes** as traffic flows through the switches.


#### Step 1 – Record the MAC Addresses

1. On **PC‑A** and **PC‑B**, open a command prompt and run:

   ```bash
   ipconfig /all
   ```

   * Note each PC’s **physical (MAC) address** on its Ethernet adapter.

2. From the console on **S1** and **S2**, run:

   ```text
   show interface f0/1
   ```

   * On the second line of the output, record the **hardware address** (also listed as the BIA –
     burned‑in address) for F0/1 on both switches.


#### Step 2 – Display the MAC Address Table (Before Traffic)

On **S2**, display the table before you generate any user traffic:

```text
S2# show mac address-table
```

* Even without pings, there may already be entries from directly connected devices or from the
  inter‑switch link.
* Identify which MAC addresses are present and to which ports they map (ignore internal CPU entries).
* If you have written down the device MAC addresses from Step 1, you can match each MAC entry to
  a device by port:
  * MAC on PC‑B’s port → PC‑B
  * MAC on F0/1 → S1 or PC‑A traffic going through S1, etc.
* Without prior notes, you could infer some mappings from the connected port (for example, the MAC
  seen on the port facing S1 likely belongs to S1 or hosts behind it), but this may not always be exact
  on larger or more complex topologies.


#### Step 3 – Clear the MAC Address Table and Watch It Rebuild

1. On S2, clear the **dynamic** entries:

   ```text
   S2# clear mac address-table dynamic
   ```

2. Immediately run:

   ```text
   S2# show mac address-table
   ```

   * Dynamic entries for VLAN 1 should now be gone. You might still see some static or system entries.

3. Wait ~10 seconds and repeat `show mac address-table`.

   * As control traffic or management frames are exchanged, new entries can start appearing again.
   * This shows how **quickly** switches relearn MAC addresses when traffic resumes.


#### Step 4 – Generate Traffic from PC‑B and Observe MAC and ARP Tables

1. On **PC‑B**, open a command prompt and display the ARP cache:

   ```bash
   arp -a
   ```

   * At this point, ARP may have few or no entries besides broadcast/multicast.

2. From PC‑B, ping all devices in the topology:

   ```bash
   ping 192.168.1.1   # PC‑A
   ping 192.168.1.11  # S1
   ping 192.168.1.12  # S2
   ```

   * Ensure all pings succeed; if not, troubleshoot cabling or IP configuration.

3. Back on S2, run:

   ```text
   S2# show mac address-table
   ```

   * You should now see **additional dynamic entries** for the MAC addresses of PC‑A, PC‑B, and S1.
   * Each entry maps a MAC address to the port where S2 learned it (e.g., PC‑B on F0/18, S1/PC‑A
     reachable via F0/1).

4. On PC‑B, run `arp -a` again.

   * The ARP cache should now contain **IP → MAC** mappings for all devices you just pinged
     (PC‑A, S1, and S2).
   * This mirrors how the switches have populated their **MAC tables** with the same MAC addresses,
     but mapped to **ports** instead of IPs.


#### Reflection – Challenges on Larger Networks

On small topologies, keeping track of MAC and ARP entries is straightforward. On larger networks you can
run into challenges such as:

* **Scale:** Switches must store thousands of MAC entries; if the table fills, older entries may be aged out
  and traffic can be flooded more often.
* **Mobility and changes:** Hosts that move between ports cause frequent table updates.
* **Troubleshooting difficulty:** With many switches and interconnections, it can be harder to trace which
  device owns a particular MAC entry.
* **Security concerns:** Attackers can exploit MAC learning (e.g., MAC flooding or spoofing) if the network
  is not properly secured.

This lab helps build intuition for reading MAC tables and understanding how frames are delivered on a
switched Ethernet network.

---

### 7.4 Switch Speeds and Forwarding Methods

Switches use their MAC address tables plus different **forwarding methods**, **buffering strategies**, and **port settings** (speed / duplex / auto-MDIX) to decide how fast and how reliably frames move through the network.

---

### 7.4.1 Frame Forwarding Methods on Cisco Switches

Cisco switches use the MAC address table to decide **which port** should forward a frame.  
There are **two main forwarding methods**:

#### Store-and-forward switching

- The switch **receives the entire frame** before forwarding.
- It calculates the **CRC** (Cyclic Redundancy Check) over all the bits to see if the frame has errors.
- If the CRC is **invalid**, the frame is **discarded** and not forwarded.
- If the frame is **valid**, the switch looks up the **destination MAC address** in the MAC table and forwards the frame out the matched port.

**Advantages:**

- Corrupted frames are dropped before they consume bandwidth on the outgoing link.
- Required for **QoS (Quality of Service)** features on converged networks, where traffic such as **VoIP** needs priority over web browsing or other best-effort data.

#### Cut-through switching

- The switch **starts forwarding** the frame **before the full frame is received**.
- It only needs to buffer enough of the frame to read the **destination MAC address**.
- Once the destination MAC is known and the outgoing port is identified, the switch forwards the frame immediately.
- There is **no CRC check**; errored frames may be forwarded.

**Main benefit:**  
Much **lower latency**, because forwarding begins as soon as the destination MAC is read.

---

### 7.4.2 Cut-Through Switching

In cut-through mode, the switch **acts on incoming data as soon as it can see the destination MAC**:

1. The frame begins arriving on the ingress port.
2. After the **preamble and SFD**, the switch reads the **first 6 bytes** of the header (destination MAC).
3. It looks up this MAC address in its switching/MAC table.
4. It immediately forwards the frame to the selected outbound port, **without waiting** for the rest of the bits.
5. The switch **does not check** the FCS for errors.

Cut-through switching has **two variants**:

#### Fast-forward switching

- Offers the **lowest latency**.
- The switch forwards the frame **as soon as the destination MAC is read**, with no additional delay.
- Errored packets can be forwarded because no error checking is done.
- Latency is measured from the **first bit received to the first bit transmitted**.
- This is the **typical** cut-through method.

#### Fragment-free switching

- The switch **stores the first 64 bytes** of the frame before forwarding.
- Most collisions and many physical-layer errors show up in the **first 64 bytes**, so checking this portion reduces the chance of forwarding corrupted frames.
- Considered a **compromise** between:
  - **Store-and-forward** (high latency, high integrity), and
  - **Fast-forward cut-through** (low latency, lower integrity).
- Provides **lower latency** than full store-and-forward, but **better error protection** than pure fast-forward.

Some switches can **dynamically change** behavior:

- Operate in cut-through mode **per port** until a user-defined **error threshold** is exceeded.
- If too many errors occur, the port automatically switches to **store-and-forward**.
- When error rates drop again, the port may revert back to **cut-through**.

---

### 7.4.3 Memory Buffering on Switches

Switches may need to **buffer frames in memory** before forwarding, for example:

- When the outgoing port is **busy or congested**.
- When frames arrive faster than they can be transmitted.

There are **two main memory buffering methods**:

#### Port-based memory

- Each port has its **own queue** (or queues) linked to that port’s incoming/outgoing traffic.
- Frames are stored in the port’s queue and transmitted **in order**:
  - An outgoing port only sends a frame when **all earlier frames** in that port’s queue have been sent.
- A single **busy destination port** can delay **all frames** in its queue, even if some frames could be sent to different, idle ports.
- Delay is tied to **per-port queues**.

#### Shared memory

- All frames go into a **common memory buffer** shared by all ports.
- Buffer space is **dynamically allocated** to ports as needed.
- A frame can be **received on one port** and then **transmitted on another** without moving between distinct per-port queues.
- This method:
  - Makes it easier to handle **bursty traffic**.
  - Allows **larger frames** to be stored with **fewer drops**.
  - Is especially useful for **asymmetric switching**, such as a **10 Gbps server** connected to many **1 Gbps** client ports.

---

### 7.4.4 Duplex and Speed Settings

Each switch port has two key settings:

- **Speed (bandwidth)** – 10, 100, 1000 Mbps, 10 Gbps, etc.
- **Duplex mode** – how data flows on the link.

It is critical that the **speed and duplex** settings on both ends of a link **match**, otherwise performance problems occur.

#### Duplex modes

- **Full-duplex**
  - Both ends of the connection can **send and receive simultaneously**.
  - No collisions on the link.
- **Half-duplex**
  - Only **one side can transmit at a time**.
  - Uses mechanisms like CSMA/CD to avoid or handle collisions.

#### Autonegotiation

- Available on most Ethernet switches and NICs.
- Two devices automatically negotiate:
  - The **highest common speed**, and
  - The **best duplex mode** (full if both support it).
- Example:  
  PC-A connects to switch S2. Both support **10 or 100 Mbps** and **half/full duplex**. With autonegotiation enabled, they choose **100 Mbps, full-duplex**.

**Important notes:**

- Most Cisco switches and Ethernet NICs **default** to autonegotiation for speed and duplex.
- **Gigabit Ethernet** ports operate **only in full-duplex**.

#### Duplex mismatch

A very common performance issue on 10/100 Mbps links:

- Occurs when **one side** of the link is configured for **full-duplex** and the **other side** is **half-duplex**.
- The full-duplex side sends whenever it wants; the half-duplex side must wait for a clear link → results in **many collisions** and poor performance.
- Causes:
  - One side reset and renegotiated, the other left fixed.
  - Manual reconfiguration on just one side.

**Best practice:**

- Either **both sides autonegotiate**, or
- **Both sides are manually set** to identical speed and duplex (usually full-duplex).

---

### 7.4.5 Auto-MDIX

Historically, you had to choose the **correct cable type**:

- **Straight-through cable**
  - Used to connect **unlike devices**:
    - Switch ↔ Host
    - Switch ↔ Router
- **Crossover cable**
  - Used to connect **like devices**:
    - Switch ↔ Switch
    - Host ↔ Host
    - Router ↔ Router  
  - A **direct router ↔ host** connection also required a crossover cable.

#### Auto-MDIX (Automatic Medium-Dependent Interface Crossover)

Modern switch ports often support **auto-MDIX**:

- The switch **automatically detects**:
  - The type of device on the other end, and
  - Whether a **straight-through** or **crossover** cable is plugged in.
- It then **reconfigures the transmit/receive pairs internally** so the link works correctly.

**Result:**

- You can plug in **either cable type** (straight-through or crossover), and the port will work.
- Auto-MDIX is supported on most switches for **10/100/1000 Mbps copper** ports.

---
### 7.4.6 Check Your Understanding – Switch Speeds and Forwarding Methods

#### Question 1  
**What are two methods for switching data between ports on a switch? (Choose two.)**

Options:  
- Cut-through switching  
- Store-and-forward switching  
- Cut-off switching  
- Store-and-supply switching  
- Store-and-restore switching  

**Correct answers:** `Cut-through switching`, `Store-and-forward switching`

**Why?**  
Cisco switches use exactly two frame forwarding methods:  

- **Store-and-forward** – the switch receives the *entire* frame, checks it with CRC, and only forwards it if there are no errors.  
- **Cut-through** – the switch begins forwarding as soon as it has read the destination MAC address, without waiting for the rest of the frame.  

The other answer choices are not real switching methods and are just distractors.

---

#### Question 2  
**Which switching method can be implemented using fast-forward switching or fragment-free switching?**

Options:  
- Store-and-forward switching  
- Store-and-restore switching  
- Cut-off switching  
- Cut-through switching  

**Correct answer:** `Cut-through switching`

**Why?**  
The two variants described in the module:  

- **Fast-forward switching**  
- **Fragment-free switching**  

are both *types of cut-through switching*. They are not store-and-forward modes, so the umbrella term that fits both is **cut-through switching**.

---

#### Question 3  
**Which two types of memory buffering techniques are used by switches? (Choose two.)**

Options:  
- Short-term memory buffering  
- Port-based memory buffering  
- Shared memory buffering  
- Long-term memory buffering  

**Correct answers:** `Port-based memory buffering`, `Shared memory buffering`

**Why?**  

- **Port-based memory buffering** – each port has its own queue; frames are stored per-port and leave in order when the port is free. A busy destination port can delay all frames in that queue.  
- **Shared memory buffering** – all frames go into a common buffer; the switch dynamically associates buffered frames with the correct outgoing port. This is good for handling bursts and asymmetric traffic.

“Short-term” and “long-term” memory buffering are not real Cisco terms used in this context.

---

#### Question 4  
**What feature automatically negotiates the best speed and duplex setting between interconnecting devices?**

Options:  
- Autobots  
- Autonegotiation  
- Autotune  
- Auto-MDIX  

**Correct answer:** `Autonegotiation`

**Why?**  
**Autonegotiation** allows two Ethernet devices to automatically agree on the highest common **speed** and **duplex** (e.g., 100 Mbps full-duplex).  

- **Auto-MDIX** automatically detects whether the cable should logically behave as crossover or straight-through (it fixes TX/RX pin roles), but it does *not* negotiate speed/duplex.  
- “Autobots” and “Autotune” are just joke distractors.


---

# 7.5 Module Practice and Quiz – Ethernet Switching

This section wraps up **CCNA – Module 7: Ethernet Switching** with a short
summary of what I learned and the graded quiz Q&A.

---

## 7.5.1 What did I learn in this module?

### Ethernet Frame

- Ethernet operates at **Layer 2 (data link)** and **Layer 1 (physical)**.
- The Ethernet standards define:
  - The **Layer 2 protocols** (LLC + MAC sublayers).
  - The **Layer 1 technologies** (signaling, media, connectors, bandwidth).
- The **MAC sublayer** is responsible for:
  - Data encapsulation into an Ethernet **frame**.
  - Ethernet addressing (source and destination MAC).
  - Error detection with **FCS** (Frame Check Sequence).
- Modern Ethernet LANs typically use **switches running in full-duplex**, which
  removes collisions.
- Main Ethernet frame fields (in order):
  1. **Preamble + Start Frame Delimiter (SFD)** – synchronization, “get ready”.
  2. **Destination MAC address** – who should receive the frame.
  3. **Source MAC address** – who sent the frame.
  4. **EtherType / Length** – which Layer 3 protocol is inside (IPv4, IPv6, ARP…)
  5. **Data** – the encapsulated Layer 3 PDU.
  6. **FCS** – CRC value used to detect errors.

---

### Ethernet MAC Address

- Number systems used in networking:
  - **Binary (base 2)** – bits.
  - **Decimal (base 10)** – human-friendly.
  - **Hexadecimal (base 16)** – compact representation of binary.
- Hex uses digits **0–9** and letters **A–F**.
- A **MAC address is 48 bits**, usually written as **12 hex digits**  
  (e.g. `00-60-2F-3A-07-BC`).
- Because **1 hex digit = 4 bits**, 48 bits → 12 hex digits (6 bytes).
- MAC addressing identifies the **physical source and destination NICs** on the
  local network segment (Layer 2 of OSI).
- The first 24 bits (first 6 hex digits) form the **OUI (Organizationally
  Unique Identifier)** assigned by the IEEE to a vendor.
  - Example: `00-60-2F` assigned to Cisco.
- The last 24 bits (last 6 hex digits) are **vendor-assigned**, unique per
  interface.
- Ethernet uses **different MAC addresses for**:
  - **Unicast** – one-to-one.
  - **Broadcast** – one-to-all on the LAN (`FF-FF-FF-FF-FF-FF`).
  - **Multicast** – one-to-many (group of interested receivers).

---

### The MAC Address Table

- A **Layer 2 switch** makes forwarding decisions **only using MAC addresses**.
- It is **protocol-agnostic** – it doesn’t care if the payload is IPv4, IPv6,
  ARP, etc.
- The switch maintains a **MAC address table** (a.k.a. **CAM table**) that maps:
  - **MAC address ↔ switch port**.
- **Learning process (source MAC):**
  - For every incoming frame, the switch reads the **source MAC** + **ingress
    port**.
  - If the MAC is **not in the table**, it adds a new entry.
  - If it **already exists**, it simply **refreshes the aging timer**  
    (default about 5 minutes).
  - If the same MAC appears on a different port, the switch **updates the
    entry** with the new port (it “moves” the host).
- **Forwarding process (destination MAC):**
  - If **destination MAC is in the table** → forward frame **out that single
    port** (filtering).
  - If **destination MAC is unknown** → **flood** out all ports except the
    incoming port (unknown unicast).
  - If **destination MAC is broadcast or multicast** → **flood** out all ports
    except the incoming port.

---

### Switch Speeds and Forwarding Methods

- Two main forwarding methods on Cisco switches:

  1. **Store-and-forward switching**
     - Switch **receives the entire frame**, runs a **CRC/FCS check**, and only
       forwards **valid** frames.
     - Looks up destination MAC after FCS is verified.
     - Better **reliability and QoS**, can drop errored frames before they
       consume bandwidth.

  2. **Cut-through switching**
     - Switch starts forwarding **as soon as it has read the destination MAC**
       (first 6 bytes after preamble), without waiting for the whole frame.
     - **Lower latency**, but **no FCS check** (errors are forwarded).
     - Two common variants:
       - **Fast-forward** – forwards immediately after destination MAC is read.
       - **Fragment-free** – buffers the first **64 bytes** to avoid
         collision-related fragments, then forwards (compromise between speed
         and integrity).

- **Memory buffering** methods:

  - **Port-based memory**
    - Each port has its own **queue**.
    - Frames exit in **FIFO order** per port.
    - A busy destination port can delay all frames in its queue, even if some
      could go to free ports.

  - **Shared memory**
    - All frames go into a **common buffer** shared by ports.
    - Buffer space is allocated dynamically to ports as needed.
    - A frame can be received on one port and transmitted on another **without
      being copied to a new queue**.
    - Better for **asymmetric switching** (different port speeds, e.g. 1 Gbps
      uplink ↔ 100 Mbps clients).

- **Duplex settings** on Ethernet links:
  - **Full-duplex** – both ends can send and receive **simultaneously**; no
    collisions.
  - **Half-duplex** – only one side sends at a time; uses contention methods
    (e.g. CSMA/CD) and is more prone to collisions.

- **Autonegotiation**
  - Allows two devices to automatically agree on **best common speed and
    duplex**.
  - Best practice: both sides should either **have autonegotiation enabled** or
    **both disabled and manually matched**.

- **Duplex mismatch**
  - One side full-duplex, the other half-duplex.
  - Causes collisions, poor performance, and weird network issues.
  - Common cause: misconfigured manual settings or failed autonegotiation.

- **Auto-MDIX (Automatic Medium-Dependent Interface Crossover)**
  - Switch port automatically detects whether the attached cable is
    **straight-through or crossover** and adjusts the pinouts internally.
  - Modern switches with auto-MDIX let you connect any copper cable to most
    Fast/Gigabit ports without worrying about crossover vs straight-through.

---

## 7.5.2 Module Quiz – Ethernet Switching (Q&A)

### Q1

**Question:**  
Which two characteristics describe Ethernet technology? (Choose two.)

**Correct answers:**

- It uses unique MAC addresses to ensure that data is sent to the appropriate destination.  
- It is supported by IEEE 802.3 standards.

**Explanation:**  
Ethernet is defined by **IEEE 802.3** and uses **48-bit MAC addresses** to
uniquely identify devices on the LAN so frames can be delivered to the right
NIC. The ring topology and 16 Mbps values are associated with **Token Ring**
(IEEE 802.5), not Ethernet.

---

### Q2

**Question:**  
What statement describes a characteristic of MAC addresses?

**Correct answer:**  
- They must be globally unique.

**Explanation:**  
MAC addresses are **hardware-level identifiers** assigned by vendors. The OUI
prefix ensures that each vendor uses a unique range so two devices should never
ship with the same MAC. A MAC is **48 bits**, not 32, and it is a **Layer 2**
address, not part of a Layer 3 PDU.

---

### Q3

**Question:**  
What is the special value assigned to the first 24 bits of a multicast MAC
address transporting an IPv4 packet?

**Correct answer:**  
- `01-00-5E`

**Explanation:**  
For IPv4 multicast, Ethernet uses a special **multicast MAC prefix** of
`01-00-5E`. The remaining 24 bits are derived from the lower bits of the IPv4
multicast address. `FF-FF-FF-FF-FF-FF` is the **broadcast MAC**, not multicast.

---

### Q4

**Question:**  
What will a host on an Ethernet network do if it receives a frame with a
unicast destination MAC address that does not match its own MAC address?

**Correct answer:**  
- It will discard the frame.

**Explanation:**  
Each NIC checks the **destination MAC** in every frame it sees. If the address
is **unicast and not equal** to its own MAC (and not a relevant multicast
group), it simply **drops the frame** and does not pass it up the stack.

---

### Q5

**Question:**  
Which network device makes forwarding decisions based on the destination MAC
address that is contained in the frame?

**Correct answer:**  
- Switch

**Explanation:**  
A **Layer 2 switch** uses its **MAC address table** to decide which **egress
port** should be used to forward a frame. Hubs and repeaters just repeat bits
to all ports, and routers make decisions based on **Layer 3 (IP) addresses**.

---

### Q6

**Question:**  
Which network device has the primary function to send data to a specific
destination based on the information found in the MAC address table?

**Correct answer:**  
- Switch

**Explanation:**  
Again, this describes exactly what a **switch** does: consult the MAC table
(learned from source MACs) and **forward frames only to the correct port**. A
router uses an IP routing table, not a MAC table; hubs and modems don’t keep
MAC tables at all.

---

### Q7

**Question:**  
Which function or operation is performed by the LLC sublayer?

**Correct answer:**  
- It communicates with upper protocol layers.

**Explanation:**  
The **LLC (Logical Link Control)** sublayer (IEEE 802.2) acts as the **interface
between Layer 2 and the upper layers**. It identifies which **network layer
protocol** (IPv4, IPv6, IPX, etc.) is encapsulated and provides a consistent
service to them. The **MAC sublayer** is the one responsible for media access
control and low-level encapsulation on the physical medium.

---

### Q8

**Question:**  
What happens to runt frames received by a Cisco Ethernet switch?

**Correct answer:**  
- The frame is dropped.

**Explanation:**  
A **runt frame** is **smaller than the minimum 64-byte Ethernet frame size**.
Cisco switches running store-and-forward check the **FCS and length**; frames
that are too short (often caused by collisions or errors) are **discarded** and
never forwarded.

---

### Q9

**Question:**  
What addressing information is recorded by a switch to build its MAC address
table?

**Correct answer:**  
- The source Layer 2 address of incoming frames.

**Explanation:**  
Switches **learn MAC addresses from the source MAC** of frames that **enter a
port**. They then associate that MAC with the ingress port in the MAC table.
Destination MACs are only used when **forwarding**, not for learning.

---

## Question 10  
**Question:** What is auto-MDIX?  

**Correct answer:**  
- A feature that detects Ethernet cable type  

**Explanation:**  
Auto-MDIX (**automatic medium-dependent interface crossover**) lets a switch port automatically detect whether the attached cable is straight-through or crossover and internally swap pairs if needed. This removes the need to choose the correct cable type manually.

---

## Question 11  
**Question:** What type of address is `01-00-5E-0A-00-02`?  

**Correct answer:**  
- An address that reaches a **specific group of hosts**  

**Explanation:**  
MAC addresses that start with `01-00-5E` are **IPv4 multicast MAC addresses**. Multicast traffic is delivered to a *group* of interested hosts, not to every host (broadcast) and not to just one host (unicast).

---

## Question 12  
**Question:** Which statement is true about MAC addresses?  

**Correct answer:**  
- The first three bytes are used by the vendor-assigned OUI.  

**Explanation:**  
A MAC address is 48 bits (6 bytes).  
- The **first 3 bytes (24 bits)** are the **OUI – Organizationally Unique Identifier**, assigned to the vendor by IEEE.  
- The **last 3 bytes** are a unique value assigned by that vendor to each NIC.

---

## Question 13  
**Question:** What are the two sizes (minimum and expected maximum) of an Ethernet frame? (Choose two.)  

**Correct answers:**  
- 64 bytes  
- 1518 bytes  

**Explanation:**  
Standard Ethernet (without VLAN tagging) defines:  
- **Minimum frame size:** 64 bytes – anything smaller is a **runt** and is dropped.  
- **Maximum expected size:** 1518 bytes – anything larger is considered a **giant** frame (unless special features like jumbo frames are used).

---

## Question 14  
**Question:** Which two functions or operations are performed by the MAC sublayer? (Choose two.)  

**Correct answers:**  
- It is responsible for Media Access Control.  
- It adds a header and trailer to form an OSI Layer 2 PDU.  

**Explanation:**  
The **MAC sublayer** of Layer 2:  
- Implements **media access control** (who can send and when – e.g., CSMA/CD, CSMA/CA).  
- Handles **framing**: adding the Layer-2 header and trailer (including source/destination MAC addresses and FCS) to create the Ethernet frame.  
The **LLC sublayer**, not MAC, is the part that mainly communicates with upper layers and identifies which Layer-3 protocol is encapsulated.

