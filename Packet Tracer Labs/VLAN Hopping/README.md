# VLAN Hopping Attacks Prevention & Layer 2 Security Hardening
## Network Topology Overview
Here is the complete network topology designed and secured in Cisco Packet Tracer:

![VLAN Hopping Attacks Prevention & Layer 2 Security Hardening Topology](STP-Attack-Prevention.png)

*Figure: Full Enterprise Network Topology VLAN Hopping Attacks Prevention & Layer 2 Security Hardening.*

---

1. Draw necessary topology, decorate and comment
2. Create VLANs and name them- one for hosts and one as NATIVE-
3. Identify access ports, assig them VLAN IDs and issue nonegotiate command on these ports.
4. Identify and configure trunk ports, assign them native VLAN and issue nonegotiate command on these ports.
5. Disable CDP in the devices.
-----------------------------------------------------------------------------------------------------------------------------------

## 1. Architectural Overview: How Attackers "Jump" VLANs
In a standard enterprise network, Virtual Local Area Networks (VLANs) isolate broadcast domains for security and performance. For example, `VLAN 10` (General Hosts/Students) should never access `VLAN 20` (Management/Servers) without passing through a Layer 3 routing device where security policies (ACLs/Firewalls) are enforced.

**VLAN Hopping** is a critical Layer 2 attack where a malicious actor attempts to bypass upper-layer security controls and send traffic directly from their local VLAN to a target private VLAN across the switch infrastructure. 

Attackers exploit two major architectural flaws in default switch configurations:

### Threat A: Switch Spoofing (Exploiting DTP)
By default, Cisco switches run **Dynamic Trunking Protocol (DTP)** to automatically negotiate link types. If an attacker connects a device running simulation or hacking tools (like *Yersinia* or *Scapy* in Kali Linux), they can send spoofed DTP negotiation frames to the switch port. The switch innocently misinterprets the attacker as another network switch and dynamically converts the link into a **Trunk Port**. Because trunk ports carry traffic for all VLANs, the attacker instantly gains an open pipe to intercept or inject data into any VLAN on the network.

### Threat B: Double Tagging (Exploiting Native VLAN 1)
This attack relies on an inherent hardware processing mechanism in 802.1Q trunking involving the **Native VLAN** (which is `VLAN 1` by default). Standard trunk interfaces forward Native VLAN frames without adding any 802.1Q encapsulation tags. 

If an attacker is connected to an access port on `VLAN 1`, they can craft a malicious packet containing **two nested 802.1Q tags**:
1. An **Outer Tag** matching the current Native VLAN (`VLAN 1`).
2. An **Inner Tag** matching the target high-security VLAN (`VLAN 20`).

When the first switch receives this frame, it strips off the outer tag because it matches the Native VLAN, and sends the frame across the trunk unencapsulated. When the next downstream switch receives the frame, it reads the remaining inner tag (`VLAN 20`) and directly delivers the malicious payload to the target isolated network.

---

## 2. Advanced Multi-Layered Defense: "Defense in Depth" Strategy
To completely eliminate VLAN Hopping and switch reconnaissance, this lab rejects single-point failure designs. Instead, it implements a **Defense in Depth** strategy combining Cisco Hardening Standards with advanced structural isolation practices.

### Core Defensive Pillars:
1. **Dynamic Trunking Elimination (`nonegotiate`):** Forcing ports into explicit static states and disabling DTP negotiation completely kills Switch Spoofing capabilities.
2. **Native VLAN Isolation & Tagging:** Moving the Native VLAN away from default `VLAN 1` to an unused, dedicated ID—and explicitly forcing 802.1Q tagging on it—destroys the Double Tagging mechanism.
3. **The Blackhole Strategy (VLAN Blackholing):** Moving all unused ports to an isolated dead-end network (`VLAN 999`) and stripping its permissions from network trunks ensures any unauthorized physical attachment yields zero network visibility.
4. **Reconnaissance Mitigation (Disabling CDP):** Stopping Cisco Discovery Protocol advertisements prevents hackers from sniffing enterprise device models, OS versions, and structural IP management maps through passive packet capture tools like Wireshark.

---

## 3. Real-World Architectural Best Practices (Design Logic)
When securing enterprise networks, security parameters must be applied intelligently based on traffic patterns and device functions:

| Security Action | Applied To | Engineering Justification |
| :--- | :--- | :--- |
| **`switchport mode access`** | Host Ports (PCs, Printers) | Hardcodes the port as a dead-end user interface, blocking it from acting as a network distribution path. |
| **`switchport mode trunk`** | Infrastructure Interconnects | Manual, explicit definition of core switch-to-switch lines. Eliminates automation mistakes. |
| **`switchport nonegotiate`** | **ALL** Interfaces (Access & Trunk) | Forces the switch to turn off DTP frame transmission entirely. Stops negotiation attacks dead in their tracks. |
| **`no cdp enable`** | Untrusted Edge Interfaces | Silences the switch on user-facing lines so it never leaks system blueprints, firmware versions, or management IPs to potential local packet sniffers. |
| **`no cdp run`**| If you want to completely silence the switch and prevent it from running CDP on ANY port globally, execute this single command in global configuration mode |
| **`vlan dot1q tag native`** | Global Switch Configuration | Forces explicit outer 802.1Q tagging even on Native VLAN traffic across trunks, breaking the Double Tagging attack logic entirely. |

---

## 4. Lab Implementation & Command Line Interface (CLI) Configuration

### Step 1: Broad Database Architecture & Isolation Creation
First, we build our logical containers. We define the user host zone, a highly isolated Native transport lane, and our ultimate security containment zone—the Blackhole.

```text
Switch(config)# vlan 10
Switch(config-vlan)# name IT

Switch(config)# vlan 99
Switch(config-vlan)# name Native_VLAN

Switch(config)# vlan 999
Switch(config-vlan)# name Blackhole
-----------------------------------------------------------------------------------------------------------------------------
Step 2: Hardening Host Access Layers

We capture user-facing segments (ports 5 to 11), assign them explicitly to the host data network, silence DTP negotiation, and shut down CDP informational leaks.

Switch0(config)# interface range fastEthernet 0/5 - 11

Switch0(config-if-range)# switchport mode access

Switch0(config-if-range)# switchport access vlan 10

Switch0(config-if-range)# switchport nonegotiate
---------------------------------------------------------------------------------------------------------------------------
Step 3: Securing Core Infrastructure Trunk Lines

We configure core switch-to-switch interfaces (port-channel). We declare it an explicit Trunk, shift the Native assignment to our isolated VLAN 99, and completely kill DTP.

Switch(config)# interface port-channel <number>

Switch(config-if)# switchport mode trunk

Switch(config-if)# switchport trunk native vlan 99

Switch(config-if)# switchport nonegotiate
------------------------------------------------------------------------------------------------------------------------------------------------
Step 5: Enforcing the "Blackhole Strategy" on Unused Ports

To secure unmonitored physical jacks throughout the building (ports 12 to 24 and gig0/1-2),we move them into our dead-end VLAN 999. Furthermore, we log into our trunk ports and explicitly block VLAN 999 from traveling over our core links.

# Locking down the unused access layer interfaces:

Switch(config)# interface range fastEthernet 0/12-24,gig0/1-2

Switch(config-if-range)# switchport mode access

Switch(config-if-range)# switchport access vlan 999

Switch(config-if-range)# switchport nonegotiate

Switch(config-if-range)# shutdown

# Pruning the Blackhole VLAN from our active Core Trunk lines:

Switch(config)# interface port-channel <number>

Switch(config-if)# switchport trunk allowed vlan remove 999
------------------------------------------------------------------------------------------------------------------------------------
Step 6:Verification Commands

To validate our security deployment and verify the complete mitigation of Layer 2 vulnerabilities, execute the following commands:

1.6 Verify DTP Disabling & Mode Status:

Switch# show interfaces fastEthernet 0/1 switchport

Expected Output: Operational Mode: static access and Negotiation of Trunking: Off. This confirms Switch Spoofing is impossible.

2.6 Verify Trunking Parameters & Native VLAN Shifts:

Switch# show interfaces trunk

Expected Output: The trunk interface must list 99 under the Native VLAN column, and 999 must be completely missing from the allowed VLAN range.

3.6 Verify CDP Device Suppression:

Switch# show cdp interface fastEthernet 0/1

Expected Output: The switch must indicate that CDP is disabled on user-facing edge ports, proving the network is completely silent against local reconnaissance.






























