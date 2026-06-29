# Enterprise Multilayer Switching: Layer 3 Operations & Routing

1. Draw necessary topology, decorate and comment --- choose 3650 13sw

2. Power on the Multilayer switches.

3. Connect cables and enable IP routing on both the multilayer switches.

4. Identify and configure layer-3 interfaces to be used- use no switchport command to make it 13 interface.

5. Assign IP addresses to the switch 13 interfaces plus the router.

6. Test ping between the two 13sw.
------------------------------------------------------------------------------------------------------------------------------------------------------------

# 1. Introduction: What is a Multilayer Switch (MLS)?

### Concept

A Multilayer Switch (MLS) is a high-performance device that bridges the gap between two networking worlds: the speed of Switching (Layer 2) and the intelligence of Routing (Layer 3).

### Evolution & The "Bottleneck" Problem

Traditional Model: Historically, Switches managed local device connections (L2), while Routers handled inter-network traffic (L3).

The Bottleneck: As enterprise networks grew, traditional routers became "bottlenecks." Because they rely on general-purpose software (CPU-based routing), they cannot process high-volume traffic at the speeds required by modern networks.

The Solution: The MLS emerged to perform routing using Hardware-based forwarding (ASIC). By moving the routing logic into the silicon hardware itself, the MLS can route data between VLANs with extremely low latency and ultra-high throughput.

# 2. When to Use Multilayer Switching
MLS is essential for Core and Distribution layers in enterprise networks:

Speed: Enables wire-speed Inter-VLAN routing without sending traffic to an external router.

Efficiency: Reduces the need for expensive high-end routers in internal network segments.

Scalability: Handles thousands of packets per second, ensuring the network remains performant even under heavy load

# 3. How the MLS Routes Between VLANs
VLANs are isolated broadcast domains that cannot communicate by default. The MLS uses SVI (Switched Virtual Interface) to overcome this.

## Key Concepts
### 1- Isolation: VLANs act like "locked rooms." A Layer 2 switch cannot see past the MAC address, keeping them isolated.

### 2- SVI (The Magic Portal): An SVI is a logical "gateway" or "window" created for each VLAN. The MLS assigns itself an IP address in every VLAN (e.g., 192.168.10.1 for VLAN 10).

### 3- The Routing Process:

De-encapsulation: When a packet hits the MLS, it strips the Layer 2 (MAC) header.

Routing Decision: It checks its internal Routing Table for the destination IP address.

Re-encapsulation: It re-wraps the packet with the destination's MAC address and pushes it out through the SVI of the target VLAN.

### MLS vs. Router
Can it replace a router?

Yes: For internal network routing and inter-VLAN traffic, the MLS outperforms a standard router due to its ASIC hardware.

No: Routers remain necessary for WAN (Wide Area Network) connections, VPNs, MPLS, and complex fiber internet services, as they offer the necessary flexibility for non-standard, high-complexity protocols.

### Hardware vs. Software (ASIC)
ASIC: A specialized chip designed solely for routing. It executes operations in physical hardware, making it exponentially faster than a router's CPU, which must process tasks sequentially through software.

# 4. Lab Implementation & Configuration
### Enable Routing:
```text
Switch(config)# ip routing
```
### Convert Port to Layer 3:
```text
Switch(config)# interface gigabitEthernet 1/0/1
Switch(config-if)# no switchport
```
### Assign IP:
```text
Switch(config-if)# ip address 192.168.1.1 255.255.255.0
Switch(config-if)# no shutdown
```
## Verification Results
After configuration, we verify the L3 routing path using the ICMP (Ping) utility.

Switch 0 Connectivity:

Switch 1 Connectivity:

# 5. Notes
### Security Integration:
You can apply ACLs (Access Control Lists) to SVIs to control traffic between VLANs, effectively performing firewall functions inside the switch.
### Default Gateway: 
For any device, the SVI acts as the Default Gateway, providing the "door" to reach other network segments.

# 6. Technical Comparison Summary

| Feature | Switch (L2) | Multilayer Switch (L3) | Router |
| :--- | :---: | :---: | :---: |
| **Primary Basis** | MAC Address | IP Address | IP Address |
| **Speed** | Very High | Very High (ASIC) | Medium (CPU/Software) |
| **Use Case** | Local segment connection | Inter-VLAN Routing | WAN / Internet Routing |




































