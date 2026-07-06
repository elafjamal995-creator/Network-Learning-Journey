# CONFIGURING STATIC NAT

1. Draw necessary topology, decorate and comment
2. Configure IP addresses to the routers and hosts and configure ospf .
3. Ping the Google server, traceroute the paths, and check if there are any translations.
4. Configure a static NAT to do 1-1 translation
5. Configure interfaces as NAT inside and outside.
6. Ping again, traceroute the paths, and check if there are any translations ..

# Network Security: Static NAT & Perimeter Defense

This guide documents the implementation of **Static NAT (Network Address Translation)** and the corresponding security layer using **Extended ACLs**. Static NAT provides a permanent one-to-one mapping between a private internal IP and a public IP, essential for hosting services like Web Servers.

---

## 1. The Engineering Concept
Static NAT is used to map an internal private IP (e.g., a server) to a specific public IP so it becomes reachable from the Internet.

### Why Static NAT?
* **Service Availability:** Ensures that external users can always reach your internal server using the same public IP.
* **Deterministic Mapping:** Unlike Dynamic NAT, the translation is permanent, allowing for consistent firewall rules and access policies.

---

## 2. Configuration Logic

### Step 1: NAT Mapping
Define the permanent translation between the local server and the public address.
```bash
# Map the internal server to the public IP
Router(config)# ip nat inside source static 192.168.20.10 200.100.50.2
```
### Step 2: Interface Roles
Identify which interface faces the private network and which faces the public ISP network.
```bash
Router(config)# interface gig0/0
Router(config-if)# ip nat inside
Router(config)# interface serial0/0/0
Router(config-if)# ip nat outside
```







