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

# 1. The Engineering Concept
Static NAT is used to map an internal private IP (e.g., a server) to a specific public IP so it becomes reachable from the Internet.

## Why Static NAT?
* **Service Availability:** Ensures that external users can always reach your internal server using the same public IP.
* **Deterministic Mapping:** Unlike Dynamic NAT, the translation is permanent, allowing for consistent firewall rules and access policies.

---

# 2. Configuration Logic

## Step 1: NAT Mapping
Define the permanent translation between the local server and the public address.
```bash
# Map the internal server to the public IP
Router(config)# ip nat inside source static 192.168.20.10 200.100.50.2
```
## Step 2: Interface Roles
Identify which interface faces the private network and which faces the public ISP network.
```bash
Router(config)# interface gig0/0
Router(config-if)# ip nat inside
Router(config)# interface gig0/1
Router(config-if)# ip nat outside
```

## Step 3:The Security Layer: Perimeter ACL
Exposing a server via NAT is a security risk. To protect the server, we apply an Extended ACL on the outside interface to permit only authorized traffic.

### ACL Implementation
We use the any keyword to allow global access, but restrict it to a specific port (e.g., HTTP/80).
```text
# Permit only HTTP traffic from anywhere to the server
Router(config)# access-list 150 permit tcp any host 8.8.8.2 eq 80

# Deny any other unauthorized traffic to the server
Router(config)# access-list 150 deny ip any host 8.8.8.2

# Allow other legitimate traffic
Router(config)# access-list 150 permit ip any any

# Apply to the outside interface (Inbound)
Router(config)# interface gig0/1
Router(config-if)# ip access-group 150 in
```
## Always use this command to verify your NAT table:
`Router# show ip nat translations`
If the table is empty, your NAT mapping is not functioning. Check your interface roles and static command syntax.

| Issue | Potential Cause | Fix |
| :--- | :--- | :--- |
| No translation occurring | Inside/Outside interfaces swapped | Ensure `ip nat inside` and `ip nat outside` are correctly applied. |
| Server unreachable | Mapping missing or incorrect | Use `show ip nat translations` to verify the entry exists. |
| Traffic blocked incorrectly | ACL logic too strict | Review `access-list 150` to ensure port 80 is permitted. |

## Conclusion:
Static NAT is a powerful tool, but it essentially removes the "shield" of private addressing. By combining it with an Extended ACL, we successfully bridge the need for public accessibility with the necessity of robust perimeter security.


