# CCNA – Data Link Layer (Module 6)

Quick-reference notes for NetAcad CCNA – **Module 6: Data Link Layer**.  
Focus is on how Layer 2 prepares data for transmission, controls access to the
medium, and wraps packets into frames for both LAN and WAN links.

I use this repo to revise:

- The **role of Layer 2** between the Network and Physical layers.
- The difference between the **LLC and MAC sublayers**.
- How **topologies** and **access methods** affect media access.
- The structure of a **data link frame** and how MAC addresses work.
- Key standards bodies (IEEE, ITU, ISO, ANSI) for the network access layer.

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

This module explains **how data actually crosses the wire (or air)**:

- How the data link layer takes a Layer 3 packet and wraps it into a **frame**
  that can use a specific physical medium.
- How **LLC and MAC** split responsibilities between software and hardware.
- How different **topologies and access methods** impact collisions, bandwidth
  sharing, and performance.
- How **Layer 2 addressing** and frame fields enable error detection and
  delivery to the correct next-hop device.

The idea is to understand what happens between the IP packet and the physical
signal, so that troubleshooting at Layer 2 actually makes sense.

---

## Module Map

- **6.0 – Introduction**  
  Why the data link layer is necessary and what this module will cover.

- **6.1 – Purpose of the Data Link Layer**  
  Main responsibilities of Layer 2, its sublayers, and the standards that define
  them.

- **6.2 – Topologies**  
  Physical vs logical layouts, WAN and LAN topologies, and media access methods
  (half/full duplex, CSMA/CD, CSMA/CA).

- **6.3 – Data Link Frame**  
  What a frame looks like, what each field does, and the difference between LAN
  and WAN framing.

- **6.4 – Module Practice and Quiz**  
  Wrap-up summary and final graded quiz for Module 6.

---

## 6.0 Introduction

### 6.0.1 Why should I take this module?

Every network has **physical media** and devices attached to it.  
Bits don’t magically flow across copper, fiber, or wireless – they need rules
about:

- How data is packaged for a specific medium.
- How devices decide **who can talk and when**.
- How errors are detected and bad data is discarded.

The data link layer exists exactly for this: it gives data the “push” it needs
to cross each link reliably, regardless of what the medium is.

### 6.0.2 What will I learn to do in this module?

After this module I should be able to:

- Describe the **job of Layer 2** in the OSI model.
- Explain the roles of the **LLC and MAC sublayers**.
- Compare common **LAN and WAN topologies**.
- Explain **half vs full duplex** and basic access control methods.
- Describe **frame structure** and the purpose of key fields.
- Recognize major **standards organizations** that define data link protocols.

---

## 6.1 Purpose of the Data Link Layer

### 6.1.1 The Data Link Layer

Layer 2 sits between the **Network layer (Layer 3)** and the **Physical layer
(Layer 1)**. In practice it:

- Receives packets (usually IPv4 or IPv6) from Layer 3 and **encapsulates** them
  into frames.
- Adds **source and destination Layer 2 addresses** (e.g. MAC addresses).
- Controls how data is **placed on and removed from** the physical medium.
- Exchanges frames between devices sharing the same medium or segment.
- Performs **error detection** using trailers (like FCS) and drops corrupted
  frames.
- Hides the details of the physical media so upper layers can stay media-agnostic.

Key idea: Layer 3 only cares about sending a packet to the next hop.  
Layer 2 worries about how to actually **deliver it over the current link**.

### 6.1.2 IEEE 802 LAN/MAN Data Link Sublayers

For LANs and MANs, IEEE 802 splits Layer 2 into two sublayers:

- **Logical Link Control (LLC – IEEE 802.2)**  
  - Sits closer to the upper layers.  
  - Adds control information to identify which Network layer protocol is inside
    the frame (IPv4, IPv6, etc.).  
  - Allows multiple Layer 3 protocols to share the same network interface and
    medium.

- **Media Access Control (MAC – e.g. IEEE 802.3, 802.11, 802.15)**  
  - Implemented in hardware (NICs, wireless cards, etc.).  
  - Handles **encapsulation**, **MAC addressing**, and **media access control**.  
  - Integrates with the chosen physical technology (Ethernet, Wi-Fi, Bluetooth,
    etc.).

LLC = talks to Layer 3 and labels the payload.  
MAC = talks to the medium and actually sends/receives frames.

The figure shows the two sublayers (LLC and MAC) of the data link layer.
<img width="913" height="744" alt="image" src="https://github.com/user-attachments/assets/0ad9fee9-cebd-483f-9b84-2c8775085e6e" />


### 6.1.3 Providing Access to Media

Each network segment along a path can use a **different medium** and a different
Layer 2 protocol. At every hop, a router will:

1. **Receive** a frame from the incoming medium.
2. **De-encapsulate** the frame to recover the Layer 3 packet.
3. **Re-encapsulate** that packet into a new frame appropriate for the outbound
   interface.
4. **Transmit** the new frame on the next physical link.

The MAC sublayer’s access method decides *how* the device gets permission to use
the medium – very important when many hosts share the same link.

### 6.1.4 Data Link Layer Standards

Unlike upper TCP/IP layers (often documented in IETF RFCs), data link and
physical standards are usually defined by engineering organizations such as:

- **IEEE** – e.g. Ethernet (802.3), Wi-Fi (802.11), Bluetooth/WPAN (802.15).
- **ITU** – many WAN and telecom standards.
- **ISO** – international standardization for many technologies.
- **ANSI** – standards for North America that often align with ISO/ITU work.

These groups define open specifications so different vendors’ NICs, switches and
routers can interoperate at Layer 2.

### 6.1.5 Check Your Understanding – Purpose of the Data Link Layer

Use this quiz section to confirm you can:

- State the **main functions** of Layer 2.
- Distinguish clearly between **LLC and MAC**.
- Explain what happens to a packet as it crosses a multi-hop path.

(You can add specific Q/A from NetAcad here later.)

---
