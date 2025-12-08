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
