# 🌐Jeremy's IT Lab - Practical Documentation & Configuration Breakdown

Welcome to the **Gemry's IT Lab** repository! This project is dedicated to documenting, analyzing, and explaining the popular practical network labs created by Engineer Ahmed El-Gemry, available on YouTube.

The main goal of this repository is not just to blindly copy and paste commands. Instead, it provides a **deep-dive explanation** for each lab, answering essential engineering questions:
* **Why did we use this specific command?**
* **What happens behind the scenes when this feature is enabled?**
* **Why did we avoid other alternative configurations?**
* **How do we troubleshoot issues based on real network behavior?**

---

## 🎯 Project Objectives

1. **Hands-on Practice:** Completing all Cisco Packet Tracer (`.pkt`) files, which include more than 75 practical labs.
2. **Clear Documentation:** Every single lab will have its own sub-folder containing a dedicated `README.md` file that includes:
   * **Topology Overview:** A visual or descriptive look at the network layout.
   * **Addressing Table:** Device IP addresses and subnets.
   * **Step-by-Step Configuration:** Clear guidelines on setting up the devices.
   * **Verification:** Analyzing `show` and `debug` commands to understand network outputs.
3. **Configuration & Troubleshooting Philosophy:** Focusing on scenarios where things go wrong, and learning how to read, track, and fix network issues to build strong analytical skills in network engineering and security.

---

## 🗺️ Labs Roadmap Overview

This series covers a comprehensive range of topics divided into four main learning phases, as organized in the lab files:

### 🔹 Phase 1: Basics, Initial Security, and VLANs (Labs 001 - 021)
* Basic router and switch security configurations.
* Setting up VLANs, Trunking, and Inter-VLAN Routing (Router-on-a-Stick).
* Securing switch ports using different Port Security modes.
* Full configuration and troubleshooting reviews (Review Lab 1).

### 🔹 Phase 2: Network Services, Static Routing, and ACLs (Labs 022 - 042)
* Secure and insecure remote management protocols (Telnet & SSH).
* Static Routing fundamentals and basic dynamic routing like RIP.
* Network security baselines (Standard, Extended, and Named Access Control Lists).
* Core network discovery and management protocols (LLDP, DHCP, DNS, Syslog, NTP).
* Network Address Translation techniques (Static NAT, Dynamic NAT, PAT) and transitioning to IPv6.

### 🔹 Phase 3: Advanced Routing and Layer 2 Protocols (Labs 043 - 062)
* Advanced Layer 2 switching and trunk control (DTP, VTP, STP, and STP Root Election).
* Combining network links for higher bandwidth (EtherChannel and EtherChannel Troubleshooting).
* Enterprise Dynamic Routing Protocols (OSPF Single & Multi-Area, EIGRP) for both IPv4 and IPv6 networks.
* Wide Area Network (WAN) encapsulation protocols (PPP with PAP and CHAP authentication).

### 🔹 Phase 4: Advanced WAN, High Availability, and Monitoring (Labs 063 - 076)
* Advanced link bundling and service provider connections (MultiLink PPP, PPPoE).
* Virtual tunneling and enterprise routing over the internet (GRE Tunnels and BGP).
* First-hop redundancy and network high availability (HSRP and HSRP Troubleshooting).
* Traffic analysis, monitoring, and network performance SLAs (SPAN, IP SLA, and advanced IPv4/IPv6 ACLs).
* Final comprehensive labs testing both advanced configuration and troubleshooting skills (Review Lab 3).

---

## 📂 Repository Structure

The repository is organized cleanly. Every lab has its own dedicated folder containing the topology file and its breakdown:

```text
Gemry-IT-Lab/
│
├── README.md                          # Main project file (This introduction and index)
│
├── 001 - Basic Router Security/
│   ├── 001 - Basic Router Security Configuration.pkt
│   └── README.md                      # Detailed breakdown of basic security commands
│
├── 005 - VLAN Configuration/
│   ├── 005 - VLAN Configuration.pkt
│   └── README.md                      # Detailed breakdown of VLANs and Access Ports
│
└── ... (All other labs up to Lab 076)
```
## 🛠️ Tools Used
* Cisco Packet Tracer: The primary tool used to run and test the simulation files (.pkt).

* Markdown: Used to format commands, configuration notes, and technical analysis in a clean, professional way.

## 🚀 Let's Get Started!
* "Networking is not about memorizing commands; it is about understanding packet flow and why each protocol exists."

* You can jump straight into the first lab folder [001 - Basic Router Security Configuration] to read the documentation and start configuring!

* If you find this repository helpful for your CCNA studies or practical skill building, feel free to support the project by leaving a ⭐ Star!




















