# CCNA – Data Link Layer (Module 6)

Quick-reference notes for NetAcad CCNA – **Module 6: Data Link Layer**.  
Focus is on how Layer 2 prepares network data for the physical medium and
controls access to that medium.

I use this repo to revise:

- The **role of the data link layer** between the Network and Physical layers.
- The **LLC and MAC** sublayers and what each one does.
- How the data link layer **provides access to media** at every hop.
- Which **standards organizations** define Layer 2 technologies.

---

## Table of Contents

- [Module 6 Overview](#module-6-overview)
- [Module Map](#module-map)

- [6.0 Introduction](#60-introduction)  
  - [6.0.1 Why should I take this module?](#601-why-should-i-take-this-module)  
  - [6.0.2 What will I learn to do in this module?](#602-what-will-i-learn-to-do-in-this-module)

- [6.1 Purpose of the Data Link Layer](#61-purpose-of-the-data-link-layer)  
  - [6.1.1 The Data Link Layer](#611-the-data-link-layer)  
  - [6.1.2 IEEE 802 LAN/MAN Data Link Sublayers](#612-ieee-802-lanman-data-link-sublayers)  
  - [6.1.3 Providing Access to Media](#613-providing-access-to-media)  
  - [6.1.4 Data Link Layer Standards](#614-data-link-layer-standards)  
  - [6.1.5 Check Your Understanding – Purpose of the Data Link Layer](#615-check-your-understanding--purpose-of-the-data-link-layer)

- [6.2 Topologies](#62-topologies)  
  - [6.2.1 Physical and Logical Topologies](#621-physical-and-logical-topologies)  
  - [6.2.2 WAN Topologies](#622-wan-topologies)  
  - [6.2.3 Point-to-Point WAN Topology](#623-point-to-point-wan-topology)  
  - [6.2.4 LAN Topologies](#624-lan-topologies)  
  - [6.2.5 Half and Full Duplex Communication](#625-half-and-full-duplex-communication)  
  - [6.2.6 Access Control Methods](#626-access-control-methods)  
  - [6.2.7 Contention-Based Access – CSMA/CD](#627-contention-based-access--csmacd)  
  - [6.2.8 Contention-Based Access – CSMA/CA](#628-contention-based-access--csmaca)  
  - [6.2.9 Check Your Understanding – Topologies](#629-check-your-understanding--topologies)

- [6.3 Data Link Frame](#63-data-link-frame)  
  - [6.3.1 The Frame](#631-the-frame)  
  - [6.3.2 Frame Fields](#632-frame-fields)  
  - [6.3.3 Layer 2 Addresses](#633-layer-2-addresses)  
  - [6.3.4 LAN and WAN Frames](#634-lan-and-wan-frames)  
  - [6.3.5 Check Your Understanding – Data Link Frame](#635-check-your-understanding--data-link-frame)

- [6.4 Module Practice and Quiz](#64-module-practice-and-quiz)  
  - [6.4.1 What did I learn in this module?](#641-what-did-i-learn-in-this-module)  
  - [6.4.2 Module Quiz – Data Link Layer](#642-module-quiz--data-link-layer)


---

## Module 6 Overview

### Goal of the module

Every network has **physical components and media** connecting devices.  
Different media (copper, fiber, wireless, serial links…) need different
information about the data so it can be accepted and moved across the physical
network.

The data link layer provides this “help”:

- It prepares network data from upper layers so it can travel over a specific
  medium.
- It handles **framing, addressing, and access control** to make sure data can
  be delivered successfully.

This module gives an overview of these factors, how they affect data, and the
protocols designed to ensure successful delivery.

---

## Module Map

From the NetAcad outline:

- **6.0 – Introduction**  
  Why this module matters and what you will learn.

- **6.1 – Purpose of the Data Link Layer**  
  What Layer 2 does, its sublayers (LLC/MAC), how it provides access to media,
  and which standards bodies define it.

- **6.2 – Topologies** *(placeholder – notes coming later)*  
- **6.3 – Data Link Frame** *(placeholder – notes coming later)*  
- **6.4 – Module Practice and Quiz** *(placeholder – notes coming later)*

---

## 6.0 Introduction

### 6.0.1 Why should I take this module?

Welcome to **Data Link Layer**!

- Every network has **physical components** and **media** connecting them.  
- Different types of media need **different information** about the data to
  accept it and move it across the network.

Think of it like this:

- A well-hit golf ball moves quickly through the air.  
- The **same ball** moves more slowly through water unless you hit it much
  harder.  
- The environment (air vs water) changes how the ball behaves.

Exactly the same for data: as it moves across different media, it needs help.
The data link layer provides that help – and the way it works depends on the
medium and the network technology in use.

This module explains those differences and how the data link layer supports
communication across networks.

### 6.0.2 What will I learn to do in this module?

From the module objectives table:

- **Purpose of the Data Link Layer**  
  Describe the **purpose and function** of the data link layer in preparing
  communication for transmission on specific media.

- **Topologies**  
  Compare the characteristics of **media access control methods** on WAN and
  LAN topologies.

- **Data Link Frame**  
  Describe the **characteristics and functions** of the data link frame.

---

## 6.1 Purpose of the Data Link Layer

### 6.1.1 The Data Link Layer

Layer 2 of the OSI model (the **data link layer**) prepares network data for the
**physical network**.

It is responsible for communication between **network interface cards (NICs)**:

- Enables upper layers (like IP) to **access the media** without knowing the
  details of the medium.
- Accepts Layer 3 packets (IPv4, IPv6, etc.) and **encapsulates** them into
  Layer 2 frames.
- Controls **how data is placed on and received from** the media.
- Exchanges frames between endpoints over the network medium.
- Receives encapsulated data, directs it to the proper upper-layer protocol, and
  strips off the Layer 2 information.
- Performs **error detection** and rejects any corrupt frames.

In short: Layer 2 is the translator between the **network layer** and the
**physical medium**.

### 6.1.2 IEEE 802 LAN/MAN Data Link Sublayers

IEEE 802 LAN/MAN standards apply to:

- Ethernet LANs  
- Wireless LANs (WLAN)  
- Wireless personal area networks (WPAN)  
- Other types of local and metropolitan area networks

The IEEE 802 data link layer is split into two **sublayers**:

- **Logical Link Control (LLC)**  
  - Communicates between networking software at the upper layers and the device
    hardware at the lower layers.  
  - Places information in the frame that identifies **which Network layer
    protocol** (IPv4, IPv6, etc.) is being used.  
  - Allows multiple Layer 3 protocols to use the **same network interface and
    media**.

- **Media Access Control (MAC)**  
  - Implemented in hardware (NICs and related components).  
  - Responsible for **data encapsulation** and **media access control**.  
  - Provides **data link addressing** and integrates with various physical layer
    technologies (Ethernet, WLAN, WPAN, etc.).

The figure in NetAcad shows the LLC and MAC sublayers stacked between the
Network and Physical layers.

### 6.1.3 Providing Access to Media

As packets travel from a local host to a remote host, they cross **different
network environments** and **different media**. Each environment can have:

- Different numbers of hosts contending for the medium.
- Different access methods (e.g. Ethernet vs serial links).

The MAC sublayer resolves these differences by using appropriate **media access
control methods**.

For each hop along the path, a router’s data link layer does four main things:

1. **Accepts a frame** from a medium.  
2. **De-encapsulates** the frame to recover the Layer 3 packet.  
3. **Re-encapsulates** the packet into a new frame suitable for the next
   medium.  
4. **Forwards** the new frame onto the appropriate physical link.

The animation in NetAcad shows a router with an Ethernet interface toward the
LAN and a serial interface toward the WAN. At each interface, the router uses
the local data link protocol to receive, strip, wrap, and send the packet on its
way.

### 6.1.4 Data Link Layer Standards

Data link and physical layer protocols are usually not defined by IETF RFCs.
Instead, **engineering organizations** define open standards and protocols that
apply to the network access layer (OSI Layers 1 and 2).

Key organizations:

- **IEEE – Institute of Electrical and Electronics Engineers**  
  Defines many LAN/WLAN/WPAN technologies (e.g. Ethernet, Wi-Fi, Bluetooth).

- **ITU – International Telecommunication Union**  
  Creates telecom and WAN standards used worldwide.

- **ISO – International Organization for Standardization**  
  Develops international standards across many technologies, including
  networking.

- **ANSI – American National Standards Institute**  
  Coordinates standards within the U.S., often aligning with ISO/ITU work.

These groups ensure that different vendors’ hardware can interoperate at the
data link and physical layers.

### 6.1.5 Check Your Understanding – Purpose of the Data Link Layer

My answers to the 6.1.5 quiz:

1. **What is another name for the OSI data link layer?**  
   → **Layer 2**

2. **The IEEE 802 LAN/MAN data link layer consists of which two sublayers?**  
   → **Media Access Control (MAC)** and **Logical Link Control (LLC)**

3. **What is the responsibility of the MAC sublayer?**  
   → **Provides the method to get the frame on and off the media**

4. **What Layer 2 function does a router perform? (Choose three.)**  
   → **Accepts a frame from a medium**  
   → **De-encapsulates the frame**  
   → **Re-encapsulates the packet into a new frame**

5. **The media access control method used depends on which two criteria?**  
   → **Media sharing**  
   → **Topology**

6. **Which organization defines standards for the network access layer (OSI
   physical and data link layers)?**  
   → **IEEE**


---

## 6.2 Topologies

### 6.2.1 Physical and Logical Topologies

The data link layer prepares network data for the **physical network**, so it has to
understand how devices are connected (topology).

- **Topology** = arrangement/relationship of network devices and their
  interconnections.
- When describing LAN/WAN networks we talk about two views:

**Physical topology**

- Shows **real-world connections** and locations:
  - Cables, switches, routers, wireless APs.
  - Rack / room numbers, which switch a device is plugged into.
- Common physical layouts: **point-to-point** or **star / extended star**.

**Logical topology**

- Shows **how frames actually travel** from one node to another.
- Based on **interfaces** and **Layer 3 addressing** (IP networks, subnets, VLANs).
- This is the topology that the **data link layer “sees”** when controlling access
  to the medium and deciding how to frame traffic.

> **Exam note:**  
> Physical = cables & hardware layout.  
> Logical = path of frames / IP networks.

---

### 6.2.2 WAN Topologies

WANs are usually built using a few common **physical WAN topologies**:

**Point-to-Point**

- Simplest and most common WAN topology.
- A **permanent link between two endpoints** (two routers).
- Easy to understand and troubleshoot.

**Hub and Spoke**

- WAN version of a **star** topology.
- A **central site (hub)** connects out to multiple **branch sites (spokes)** using
  point-to-point links.
- Branches **cannot** communicate directly with each other; they must go **through
  the hub**.

**Mesh**

- Each site is **interconnected with every other site**.
- Provides **high availability** and redundancy.
- Admin and physical costs are **high** (lots of links).
- In practice you often see **partial mesh / hybrid** topologies where only some
  sites have full-mesh links.

---

### 6.2.3 Point-to-Point WAN Topology

Physical point-to-point topologies connect **two nodes directly**.

- The two nodes do **not share the medium** with other hosts.
- Often used with serial links and protocols like **PPP**.
- Because frames can only travel between the two nodes, **data link protocols can
  be simple**.

Notes from the figure/text:

- The node **places frames** on the media at one end and **removes frames** at the
  other end.
- Point-to-point topologies are **limited to two nodes**, but:
  - Physically you may add intermediate devices in the provider’s cloud.
  - Logically it is **still point-to-point** between the customer nodes.
- A point-to-point link over Ethernet means the device must still **check the
  destination** of each incoming frame to see if it’s meant for this node.

---

### 6.2.4 LAN Topologies

In multiaccess Ethernet LANs, end devices (nodes) are usually interconnected using
**star** or **extended star** topologies.

**Star topology**

- End devices connect to a **single central device**, typically a switch.
- Easy to install, scale (add/remove devices), and troubleshoot.

**Extended star**

- Expands the star by **interconnecting multiple switches**.
- Very common in modern LANs (access → distribution → core).
- Still easy to scale and manage.

**Point-to-point in LANs**

- Sometimes only **two devices** are connected, e.g. two routers back-to-back.
- Ethernet can then be used in a **point-to-point** role.

**Legacy LAN topologies**

- **Bus**
  - All end systems chained along a single cable and **terminated at both ends**.
  - No switches required; early Ethernet using coax.
  - Inexpensive and simple, but not scalable and hard to troubleshoot.
- **Ring**
  - End systems connected to their neighbours in a **closed loop**.
  - No termination needed.
  - Used by older technologies such as **FDDI** and **Token Ring**.

Figures for physical topologies: **Star, Extended Star, Bus, Ring**.

> **Exam note:** Modern Ethernet LANs = star / extended star.  
> Bus and ring are mainly **legacy** but still appear in exam questions.

---

### 6.2.5 Half and Full Duplex Communication

Understanding **duplex** is important when talking about LAN topologies, because it
describes the *direction* in which data can flow between two devices on a link.

There are two common duplex modes:

**Half-duplex communication**

- Both devices can **transmit and receive**, but **not at the same time**.
- Only **one device** is allowed to send on the shared medium at any moment.
- Common on:
  - WLANs
  - Legacy Ethernet **bus topologies** using **Ethernet hubs**
- If one side is sending, the other must wait before it can transmit.

**Full-duplex communication**

- Both devices can **transmit and receive simultaneously** on the link.
- The data link layer assumes the medium is always available for both nodes.
- **Ethernet switches** operate in full-duplex by default.
- A switch port may fall back to **half-duplex** if it connects to a legacy hub.

**Key idea**

- Half-duplex **restricts** data exchange to **one direction at a time**.  
- Full-duplex allows **send and receive at the same time** on the same link.
- It is important that **both connected interfaces** (e.g., NIC ↔ switch port) use
  the **same duplex setting**; a **duplex mismatch** causes inefficiency and
  latency on the link.

---

### 6.2.6 Access Control Methods

Ethernet LANs and WLANs are examples of **multiaccess networks**.  
A multiaccess network is a network where **two or more end devices** try to use the
same physical medium at the same time.

Some multiaccess environments need rules to decide **how devices share** the
medium. For shared media there are **two basic access control methods**:

- **Contention-based access**
- **Controlled access**

**Contention-based access**

- All nodes operate in **half-duplex**, **competing** for the right to send.
- Only **one device** can send at a time; if more than one transmits, there is a
  collision.
- Examples:
  - **CSMA/CD** – Carrier Sense Multiple Access with Collision Detection  
    Used in **legacy bus-topology Ethernet LANs** and Ethernet hubs.
  - **CSMA/CA** – Carrier Sense Multiple Access with Collision Avoidance  
    Used in **wireless LANs (WLANs)**.

**Controlled access**

- Each node gets its **own time slot** to use the medium.
- Access is **deterministic** (order is controlled), but this is less efficient
  because every device must **wait for its turn**.
- Examples of legacy networks that used controlled access:
  - **Token Ring**
  - **ARCNET**

> Note: Modern switched Ethernet networks operate in **full-duplex** and **no longer
> require an access method** like CSMA/CD on each link.

---

### 6.2.7 Contention-Based Access – CSMA/CD

**CSMA/CD** (Carrier Sense Multiple Access with Collision Detection) is a
contention-based access method used on older **half-duplex Ethernet** networks.

Examples of networks that use contention-based methods:

- **Wireless LAN** – uses **CSMA/CA** (collision avoidance).
- **Legacy bus-topology Ethernet LAN** – uses **CSMA/CD**.
- **Legacy Ethernet LANs using hubs** – also use **CSMA/CD**.

These networks share one medium, so only one device can send at a time. If **two
devices transmit simultaneously**, a **collision** occurs.

**How CSMA/CD works (legacy Ethernet with a hub)**

1. **PC1 wants to send a frame to PC3**
   - PC1’s NIC first **listens to the medium** (carrier sense) to check if anyone
     else is transmitting.
   - If the medium is idle, PC1 **sends the Ethernet frame**.

2. **The hub receives the frame**
   - The hub is a **multiport repeater**: it regenerates the bits and **forwards**
     them out **all other ports**.
   - If PC2 wants to send while it is still **receiving** this frame, it must
     **wait** until the channel is clear.

3. **The hub sends the frame to all devices**
   - Every attached device receives the bits, but only the device whose NIC sees
     its **own data link address** (PC3) **accepts and copies** the full frame.
   - Other devices (e.g., PC2) **ignore** the frame because it is not addressed
     to them.

If a collision is detected (signal level or data does not match), the devices stop,
wait a random backoff time, and then **retransmit**.

---

### 6.2.8 Contention-Based Access – CSMA/CA

Another form of CSMA used by **IEEE 802.11 WLANs** is **CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)**.

- CSMA/CA uses a method similar to CSMA/CD to check if the medium is free, but it adds **extra mechanisms** to help avoid collisions.
- In a wireless environment it may not be possible to reliably detect collisions on the air.
- Instead of detecting collisions, CSMA/CA tries to **avoid** them:
  - A device waits for a clear channel before transmitting.
  - Each transmission includes a **time duration** field indicating how long the medium will be in use for that frame.
  - All other wireless devices that hear the frame learn how long the medium will be unavailable and defer their own transmissions for that period.

In the example figure, if host **A** is receiving a wireless frame from the access point, hosts **B** and **C** also see the frame and know how long the channel will be busy.

After a wireless device sends an 802.11 frame, the receiver sends an **acknowledgment (ACK)** so that the sender knows the frame arrived correctly.

> Note: Whether it is an Ethernet LAN using hubs or a WLAN, contention-based systems **do not scale well** under heavy use, because many devices are competing for the same medium.  
> Ethernet LANs built with **switches** are different: switches and host NICs operate in **full-duplex**, so modern switched Ethernet does **not** use a contention-based access method.

---

### 6.2.9 Check Your Understanding – Topologies (Quiz Q&A)

**Q1. Which topology displays networking device Layer 3 IP addresses?**  
**A:** Logical topology

**Q2. What kind of network would use point-to-point, hub and spoke, or mesh topologies?**  
**A:** WAN

**Q3. Which LAN topology is a hybrid topology?**  
**A:** Extended star

**Q4. Which duplex communication method is used in WLANs?**  
**A:** Half-duplex

**Q5. Which media access control method is used in legacy Ethernet LANs?**  
**A:** Carrier sense multiple access / collision detection (**CSMA/CD**)


---

## 6.3 Data Link Frame

### 6.3.1 The Frame

Here the focus shifts from “which path do we use?” to “how do we actually move bits over this link?”.

- The **data link layer** takes a Layer 3 PDU (usually an IPv4 or IPv6 packet) and
  wraps it in a **frame** that can travel over the local medium.
- Every data link protocol (Ethernet, WLAN, PPP, etc.) defines its own frame
  format, but each frame always has three basic parts:
  **header**, **data**, and **trailer**.
- The header and trailer contain **control information** used for delivery on the
  local link: addressing, type of payload, QoS, error detection, etc.
- There is **no single universal frame format**. Links that are “fragile”
  (wireless, satellite, noisy environments) need more control information, so
  their headers/trailers are larger, which increases overhead and can reduce
  throughput.
- Data link protocols are responsible for **NIC-to-NIC communication inside the
  same network**, not for end-to-end routing across multiple networks.

---

### 6.3.2 Frame Fields

Framing breaks the continuous bit stream into recognizable chunks with structure
that devices can understand.

Generic frame fields:

- **Frame start / Frame stop indicator flags** – Mark the beginning and end of
  the frame so the receiver knows where to start and stop reading.
- **Addressing** – Source and destination **Layer 2 (physical/MAC) addresses**
  on the medium.
- **Type** – Identifies which **Layer 3 protocol** is carried in the data field
  (for example IPv4 vs IPv6).
- **Control** – Optional field for flow control and special services such as
  **quality of service (QoS)**.
- **Data** – The actual payload (the encapsulated network-layer packet or any
  other upper-layer data).
- **Error Detection** – Trailer field used for **error checking**, usually a
  **Frame Check Sequence (FCS)** value computed with a **cyclic redundancy check
  (CRC)**.

On transmit, the sender calculates the CRC over the frame contents and places
the result in the FCS. On receive, the NIC recalculates the CRC and compares it
with the FCS to decide whether the frame was damaged in transit.

---

### 6.3.3 Layer 2 Addresses

Layer 2 addresses are how frames find their way across a **single local link**.

- Data link addressing is often called **physical addressing** and usually means
  **MAC addresses**.
- The destination MAC address is placed at the **start of the frame header** so
  the NIC can quickly decide whether to accept or ignore the frame. The header
  also carries the **source** MAC address.
- Unlike Layer 3 (IP) addresses, Layer 2 addresses:
  - **Do not indicate the network** where a device lives.
  - **Do not change** if the device is moved to another IP network.
  - Are **only valid locally** on the current link or LAN segment.

As an IP packet travels:

- **Host-to-Router** – The host encapsulates the IP packet in a frame whose
  source MAC is the host NIC and destination MAC is the default gateway (R1).

  <img width="758" height="414" alt="image" src="https://github.com/user-attachments/assets/9fff9b8b-8f72-4449-ae20-15be05d6f71d" />
  

- **Router-to-Router** – Each router de-encapsulates the incoming frame, looks at
  the **IP header** to decide the next hop, then re-encapsulates the IP packet
  in a **new frame** with new source/destination MAC addresses for the next link.

  <img width="761" height="403" alt="image" src="https://github.com/user-attachments/assets/e9bfcffd-1be6-4fa4-8bbe-58ae0f3ef977" />
  

- **Router-to-Host** – On the final link, the router sends a frame whose
  destination MAC is the target host and whose source MAC is the router’s
  outgoing interface.

  <img width="780" height="411" alt="image" src="https://github.com/user-attachments/assets/10bd5622-e1d4-4cd3-ac4c-b7134d0d487c" />


Key idea:  
**Layer 3 addresses stay the same from source to final destination, while Layer 2
addresses are rewritten at every hop for local delivery.**

---

### 6.3.4 LAN and WAN Frames

Ethernet protocols are used by wired LANs, while wireless LANs use WLAN (IEEE 802.11) protocols. These were designed for multiaccess networks where many devices share the same medium.

WANs have traditionally used a range of other Layer 2 protocols for point-to-point, hub-and-spoke, and full-mesh topologies. Common examples are:

- Point-to-Point Protocol (PPP)  
- High-Level Data Link Control (HDLC)  
- Frame Relay  
- Asynchronous Transfer Mode (ATM)  
- X.25  

These legacy WAN protocols are now increasingly being replaced by Ethernet in the WAN.

In a TCP/IP network, all Layer 2 protocols work with IP at OSI Layer 3. The specific Layer 2 protocol in use depends on the logical topology and the physical media. NICs, router interfaces, and Layer 2 switches all act as nodes running the appropriate data link protocol on each link.

The technology chosen (and therefore the Layer 2 protocol) is influenced by the size and geography of the network, the number of hosts, and the services that must be provided. LANs usually use high-bandwidth technologies to support many users in a relatively small area. WANs must span large geographic areas, so high-bandwidth technologies are often less cost-effective and lower bandwidth links are used.

Data link layer protocols you should recognise include:

- Ethernet  
- 802.11 Wireless  
- Point-to-Point Protocol (PPP)  
- High-Level Data Link Control (HDLC)  
- Frame Relay  

---

### 6.3.5 Check Your Understanding – Data Link Frame

**Question 1 – What does the data link layer add to a Layer 3 packet to create a frame? (Choose two.)**  
Header and trailer.

**Question 2 – What is the function of the last field in a data link layer frame?**  
To determine whether the frame experienced transmission errors.

**Question 3 – Which lists the Layer 2 and Layer 3 address fields in the correct order?**  
Destination NIC address, source NIC address, source IP address, destination IP address.

**Question 4 – Which of the following are data link layer protocols? (Choose three.)**  
802.11, PPP, and Ethernet.

---

### 6.4 Module Practice and Quiz

---

### 6.4.1 What did I learn in this module?

**Purpose of the Data Link Layer**

The data link layer of the OSI model (Layer 2) prepares network data for the physical network. It is responsible for NIC-to-NIC communications on the same network and for getting frames on and off the medium. Data link protocols (for example the IEEE 802 LAN/MAN standards) define how each type of media is accessed, including frame delimiting, addressing, and error detection. Router and switch interfaces encapsulate Layer 3 packets into the appropriate Layer 2 frame. Standards bodies that define open standards and protocols for the network access layer include IEEE, ITU, ISO, and ANSI.

**Topologies**

LAN and WAN networks use both **physical** and **logical** topologies.  
The data link layer “sees” the logical topology when controlling access to the media. Logical topology influences the type of network framing and media-access control used.

Common **physical WAN** topologies: point-to-point, hub-and-spoke, and mesh.  
Physical **LAN** topologies: star, extended star, bus, and ring.

- In multiaccess LANs, nodes connect to a central device (usually a switch) using star or extended-star physical topologies.
- Half-duplex communication lets devices send and receive data, but not at the same time. Full-duplex allows simultaneous send/receive and requires both ends of a link to use the same duplex mode.
- Ethernet LANs and WLANs are examples of multi-access networks, where multiple nodes share the same medium.  
  - **Contention-based** access (CSMA/CD for legacy Ethernet, CSMA/CA for WLANs) lets many devices compete for the medium.  
  - **Controlled** access (such as legacy Token Ring or ARCNET) gives each node a turn to use the medium.

**Data Link Frame**

The data link layer encapsulates the Layer 3 packet (usually IPv4/IPv6) for transport over the local medium by adding a **header** and **trailer** to create a frame.

- There is no single universal frame format; each data link protocol defines its own fields to match the media and topology.
- All frame types include three basic parts: **header**, **data**, and **trailer**.

Generic frame header/trailer fields:

- **Frame start/stop flags** – mark the beginning and end of the frame.
- **Addressing** – source and destination data link (Layer 2) addresses; used only for local delivery on the shared medium.
- **Type** – identifies the Layer 3 protocol inside the data field (for example IPv4 vs IPv6).
- **Control** – may signal special handling such as QoS.
- **Data** – the encapsulated Layer 3 packet (header + payload).
- **Error detection** – trailer field (for example FCS/CRC) that lets the receiver detect whether the frame experienced transmission errors.

Layer 2 addresses are physical addresses used only within the local network segment. As the packet moves host-to-router, router-to-router, and router-to-host, each hop re-encapsulates the same Layer 3 packet into a new Layer 2 frame with new source and destination MAC addresses appropriate for that link.

Data link layer protocols include Ethernet, 802.11 Wireless, PPP, HDLC, and Frame Relay.

---

### 6.4.2 Module Quiz – Data Link Layer (Q1–Q14)

---

#### Question 1  
**What identifier is used at the data link layer to uniquely identify an Ethernet device?**

**Correct answer:** `MAC address`

**Why?**  
A **MAC address** is the physical address burned into the NIC and is used at **Layer 2** to uniquely identify Ethernet devices on the local network.

---

#### Question 2  
**What attribute of a NIC would place it at the data link layer of the OSI model?**

**Correct answer:** `MAC address`

**Why?**  
The presence of a **MAC address** is what ties the NIC to **Layer 2 (data link)**. IP addresses belong to Layer 3, ports belong to Layer 4.

---

#### Question 3  
**Which two engineering organizations define open standards and protocols that apply to the data link layer? (Choose two.)**

**Correct answers:**  
- `Institute of Electrical and Electronics Engineers (IEEE)`  
- `International Telecommunication Union (ITU)`

**Why?**  
For the data link layer, standards are largely defined by **IEEE** (e.g., 802.x) and **ITU**. (Others like IANA or ISOC have roles at different layers.)

---

#### Question 4  
**What is true concerning physical and logical topologies?**

**Correct answer:** `Logical topologies refer to how a network transfers data between devices.`

**Why?**  
- **Physical topology** = how devices and cables are physically arranged.  
- **Logical topology** = **how data flows** between devices (who talks to whom and in what pattern).

---

#### Question 5  
**What method is used to manage contention-based access on a wireless network?**

**Correct answer:** `CSMA/CA`

**Why?**  
802.11 WLANs use **CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)** because wireless devices **cannot reliably detect collisions**. They try to **avoid** them using waiting/backoff and acknowledgments.

---

#### Question 6  
**A technician has been asked to develop a physical topology for a network that provides a high level of redundancy. Which physical topology requires that every node is attached to every other node on the network?**

**Correct answer:** `Mesh`

**Why?**  
In a **mesh topology**, every node has a direct link to every other node. This creates **maximum redundancy** – multiple paths exist if one link fails.

---

#### Question 7  
**Which statement describes the half-duplex mode of data transmission?**

**Correct answer:** `Data that is transmitted over the network flows in one direction at a time.`

**Why?**  
**Half-duplex** means both sides can send, but **not simultaneously**. They must take turns using the medium.

---

#### Question 8  
**Which is a function of the Logical Link Control (LLC) sublayer?**

**Correct answer:** `To identify which network layer protocol is being used`

**Why?**  
The **LLC sublayer** tells the data link layer **which Layer-3 protocol** (IPv4, IPv6, etc.) is encapsulated so it can be handed to the correct upper-layer process.

---

#### Question 9  
**Which data link layer media access control method does Ethernet use with legacy Ethernet hubs?**

**Correct answer:** `CSMA/CD`

**Why?**  
Legacy hubs create a **shared half-duplex segment**. Ethernet on such media uses **CSMA/CD (Collision Detection)** to detect collisions when two devices transmit at the same time.

---

#### Question 10  
**What are the two sublayers of the OSI model data link layer? (Choose two.)**

**Correct answers:**  
- `MAC`  
- `LLC`

**Why?**  
The data link layer is split into:  
- **MAC (Media Access Control)** – addressing, media access.  
- **LLC (Logical Link Control)** – identifies the upper-layer protocol and manages control information.

---

#### Question 11  
**Which layer of the OSI model is responsible for specifying the encapsulation method used for specific types of media?**

**Correct answer:** `Data link`

**Why?**  
The **data link layer** defines **frame formats** and encapsulation methods (Ethernet frame, 802.11 frame, PPP frame, etc.) for each type of physical media.

---

#### Question 12  
**What type of physical topology can be created by connecting all Ethernet cables to a central device?**

**Correct answer:** `Star`

**Why?**  
With all devices connected to a **single central device** (switch), the physical layout is a **star topology**.

---

#### Question 13  
**What are two services performed by the data link layer of the OSI model? (Choose two.)**

Options (paraphrased):  
1. Builds a MAC address table  
2. Determines the path to forward packets  
3. Provides media access control and performs error detection  
4. Fragments data packets into the MTU size  
5. Accepts Layer 3 packets and encapsulates them into frames  

**Correct answers:**  
- `3. Provides media access control and performs error detection.`  
- `5. Accepts Layer 3 packets and encapsulates them into frames.`

**Why?**  
Layer 2 is responsible for:  
- **Media access control + error detection** (FCS/CRC)  
- **Encapsulating Layer-3 packets into frames** for transmission.  
MAC tables and routing decisions are device-specific (switch/router), not generic L2 services.

---

#### Question 14  
**Although CSMA/CD is still a feature of Ethernet, why is it no longer necessary?**

**Correct answer:** `The use of full-duplex capable Layer 2 switches`

**Why?**  
Modern Ethernet uses **full-duplex, switch-based links**. Each link is point-to-point with no shared medium, so **collisions cannot occur**. Because there are no collisions, **CSMA/CD’s collision detection is effectively unnecessary**, even though it remains in the standard.
