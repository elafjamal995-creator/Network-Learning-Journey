Default VLAN vs Native VLAN
How to configure native VLANs Using Packet Tracer
Configuring 802.1Q Native VLANs Using Packet Tracer
How to configure switchport trunk native VLAN
--------------------------------------------------------
1. Default VLAN vs. Native VLAN
Understanding the difference between the Default VLAN and the Native VLAN is essential for proper network design. Here is a quick comparison:

Default VLAN
Core Concept:The VLAN to which all switch ports are assigned by default when the switch boots up for the first time.
Default ID:Always VLAN 1.
Modifiability:Cannot be deleted or renamed.

Native VLAN
Core Concept:The specific VLAN designated across Trunk links to carry traffic that does not have a tag (Untagged Traffic).
Default ID:VLAN 1 by default (but changing it is a critical security best practice).
Modifiability:Can be changed to any available VLAN ID (e.g., VLAN 99).

The Core Mechanism of the Native VLAN:
When standard data moves across a Trunk link, the 802.1Q encapsulation protocol adds a "label" or a Tag to every frame to tell the receiving switch which VLAN the frame belongs to.
However, data belonging to the Native VLAN is sent across the Trunk link completely untagged. When the receiving switch encounters an untagged frame, it automatically assumes it belongs to the Native VLAN and forwards it accordingly.
----------------------------------------------------------
2. Why Do We Need a Native VLAN and Why Change It?
1-Backward Compatibility with Legacy Devices: In the past, networks used legacy, unmanaged devices called Hubs. Hubs do not understand the concept of 802.1Q VLAN tags and will drop any frame containing a tag. The Native VLAN solved this by allowing switches to transmit data without tags over shared links.
2-Modern Management Protocols: Although Hubs have become obsolete, modern network infrastructure still relies on the Native VLAN because critical Cisco management protocols—such as CDP (Cisco Discovery Protocol), DTP (Dynamic Trunking Protocol), and PAgP—transmit their control packets as Untagged frames by default.
3-Security Importance (Mitigating VLAN Hopping): Leaving the Native VLAN as its factory default (VLAN 1) exposes the network to a dangerous exploit called a VLAN Hopping Attack. Threat actors can exploit the default state to "hop" from a user port to management or server VLANs without going through a router. Changing the Native VLAN to an unused, isolated ID (like VLAN 99) eliminates this vulnerability.
---------------------------------------------------------------
3. Practical Steps: Configuring 802.1Q Native VLANs
Step 1: Create the Dedicated Native VLAN on Both Switches
First, log into both switches, enter Global Configuration Mode, and create the VLAN designated for Native/Management traffic:
Switch(config)# vlan 99
Switch(config-vlan)# name Native_Management
Step 2: Configure the Trunk Port and Change the Native VLAN
Navigate to the physical interface connecting the two switches (e.g., FastEthernet 0/1):
Switch(config)# interface f0/1
Switch(config-if)# switchport mode trunk
Note:The command above explicitly turns this interface into a "Trunk", meaning it acts as a pipe allowing all VLANs to pass through using 802.1Q tags to separate them.

Next, modify the default native VLAN assignment on this specific trunk port:
Switch(config-if)# switchport trunk native vlan 99
Note:This line overrides the factory setting.You are instructing the switch: "From now on, if you receive or transmit any untagged message across this trunk link, map it to VLAN 99 instead of the default VLAN 1."
[!WARNING]
Native VLAN Mismatch Warning: > Immediately after executing this command on the first switch, your console will start generating continuous warning messages (CDP Syslog Messages) stating Native VLAN Mismatch. This is normal.
Switch 1 is now expecting VLAN 99 as its native path, while Switch 2 is still operating on default VLAN 1. To resolve this conflict and stabilize the link, you must immediately apply the exact same configuration to the opposing trunk interface on the second switch.
---------------------------------------------------------------------------------
4. Verification Commands
To verify your configuration and ensure that the Trunk link is operating securely with the correct matching native parameters, return to Privileged EXEC mode (Switch#) and run:
Switch# show interfaces trunk
Expected Output Verification:
Look closely at the output table under the Native vlan column. It should explicitly state 99 on both ends of the connection, confirming a successful and secure deployment.
