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

- 6.2 Topologies *(to be filled in)*  
- 6.3 Data Link Frame *(to be filled in)*  
- 6.4 Module Practice and Quiz *(to be filled in)*

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

This quiz section in NetAcad checks whether you can:

- Identify the **functions** of the data link layer.
- Distinguish between **LLC** and **MAC** responsibilities.
- Explain how routers handle frames at **each hop** across different media.

(Full Q&A will be added here after I complete the check-your-understanding quiz.)

---

## 6.2 Topologies

*(Section headings only – detailed notes will be added later.)*

- 6.2.1 Physical and Logical Topologies  
- 6.2.2 WAN Topologies  
- 6.2.3 Point-to-Point WAN Topology  
- 6.2.4 LAN Topologies  
- 6.2.5 Half and Full Duplex Communication  
- 6.2.6 Access Control Methods  
- 6.2.7 Contention-Based Access – CSMA/CD  
- 6.2.8 Contention-Based Access – CSMA/CA  
- 6.2.9 Check Your Understanding – Topologies  

---

## 6.3 Data Link Frame

*(Placeholder – to be filled in as we go through the screenshots.)*

---

## 6.4 Module Practice and Quiz

*(Placeholder – will be completed after doing the Module 6 quiz.)*
