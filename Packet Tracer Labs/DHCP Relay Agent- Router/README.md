# CONFIGURING DHCP RELAY AGENT ON CISCO ROUTER #######

1. Draw necessary topology, decorate and comment
2. Configure IP to the router interface and static IP addresses to the server
3. On the DHCP Server device, create DHCP pools, subnet mask, no of useable IP, default gateway and dns address.
4. Go to the router interface connecting the LAN and configure it to relay DHCP messages to the DHCP server.
5. Go to every PC and change option to DHCP.
[topology](top.png)
## Understanding DHCP Relay Agent (IP Helper Address)

This document provides a clear explanation of how DHCP Relay works, why it is essential in modern network environments, and how to implement it on Cisco devices.

## The Problem: Broadcast Limitations
The DHCP protocol relies on **Broadcast** messages to discover servers and request IP addresses. 
* **The Rule:** Broadcast messages cannot cross network boundaries (they are stopped by Routers and Layer 3 Switches).
* **The Challenge:** If a DHCP Server is located in a different subnet (e.g., VLAN 20) than the client (e.g., VLAN 10), the DHCP DISCOVER broadcast will never reach the server, leaving the client without an IP address.

## The Solution: DHCP Relay Agent
The **DHCP Relay Agent** (known as `ip helper-address` on Cisco devices) acts as a bridge or a "translator" between the client's subnet and the remote DHCP server.

### How It Works (The 3-Stage Process)
1.  **Listening:** The Router/L3 Switch listens for DHCP broadcast messages on the local interface.
2.  **Relaying (Unicast Conversion):** Instead of dropping the broadcast, the device encapsulates it and forwards it as a **Unicast** packet directly to the DHCP Server's IP address.
3.  **Returning:** When the DHCP server replies, the Router/Switch receives the response and forwards it back to the specific client on the local subnet.

### The "Smart" Relay: GIADDR
The most critical part of this process is that the Router adds the **GIADDR** (Gateway IP Address) field to the packet.
* The Router inserts the IP address of its own interface (the one that received the client's request) into the DHCP packet.
* This allows the DHCP Server to identify exactly which subnet the request originated from, enabling it to assign an IP address from the correct **Scope/Pool**.

## Implementation Guide (Cisco CLI)
In a topology where the client resides on a LAN interface (e.g., `Gig0/1`) and the server is on a different network, follow these steps on the Router:

1. **Access Configuration Mode:**
```text
# Select the Interface facing the Clients:
 Router(config)# interface GigabitEthernet0/1
# Configure the IP Helper Address:
Router(config-if)# ip helper-address 192.168.20.X  
# Replace 192.168.20.X with the actual IP address of your DHCP Server
```
## Verification & Troubleshooting
After configuring the relay agent, it is essential to verify that the packets are being relayed correctly:

### 1- Check Interface Configuration:
Use this command to ensure the helper-address is correctly applied to the interface:

```text
Router# show ip interface GigabitEthernet0/1
* Look for the line that says: "Helper address is 192.168.20.X"
```
### 2- Observe Traffic (Simulation Mode):
In Cisco Packet Tracer, switch to Simulation Mode and filter for DHCP packets. You should see:

* A Broadcast packet hitting the Router's interface.

* The Router converting it into a Unicast packet destined for the Server.

### 3- Verify IP Assignment:
On the client PC, run the following command in the Command Prompt to confirm the IP was received:
```text
ipconfig /renew
ipconfig /all
```
[test](test.png)

## Key Benefits
* Centralization: You can manage all your IP scopes from a single central DHCP Server for the entire enterprise.

* Efficiency: Eliminates the need to deploy a separate DHCP server for every floor, department, or VLAN.

* Organization: Simplifies network administration by providing a clean, centralized point of control.



   
