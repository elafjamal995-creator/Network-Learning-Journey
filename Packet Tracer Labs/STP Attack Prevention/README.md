### STP Portfast, BPDU Guard, Root Guard Configuration | STP Attacks Prevention ###

1. Draw necessary topology, decorate and comment
2. Identify trunk and configure them.N/B -- IDENTIFY ROOT BRIDGE FIRST
3. Identify non-trunk ports or access ports (on sw0&sw1) and configure them with portfast and BPDU guard.
4. Configure root guard between sw2 and sw3 to prevent sw3 from becoming the root bridge.
5. Display spanning-tree portfast information on sw0&sw1.
----------------------------------------------------------------------------------------------------
================================================================================

LAB: HARDENING LAYER 2 SPANNING TREE PROTOCOL (STP) AGAINST ATTACKS

================================================================================

--------------------------------------------------------------------------------
1. THE ARCHITECTURAL SECURITY THREAT: STP ATTACKS
--------------------------------------------------------------------------------
By default, the Spanning Tree Protocol (STP) is a trusting protocol. Switches 
elect a "Root Bridge" (the central manager of the network) based on the lowest 
Bridge ID (Priority + MAC Address). 

If a malicious actor connects a rogue switch or launches an attack tool from a 
Kali Linux machine inside a student lab , they can broadcast 
spoofed STP packets called BPDUs with a Priority of 0. 

The Impact:
The entire network infrastructure will immediately elect the hacker's machine 
as the new Root Bridge. All enterprise data traffic will be redirected through 
the attacker's device, causing a catastrophic Man-in-the-Middle (MitM) attack 
where sensitive user credentials and traffic are intercepted.

To prevent this architectural flaw, we implement a proactive, multi-layered 
defense architecture: PortFast, BPDU Guard, and Root Guard.

--------------------------------------------------------------------------------
2. TECHNICAL CONCEPTS
--------------------------------------------------------------------------------

* PortFast:
  Optimizes end-user interfaces (PCs, Printers) by bypassing the standard 30-50 
  second STP transition delays (Listening/Learning). It safely transitions the 
  port into the Forwarding State instantly (in less than a second).

* BPDU Guard (The PortFast Bodyguard):
  End-user machines NEVER generate or send STP BPDU packets. Therefore, if a 
  PortFast interface receives a BPDU packet, it means a rogue switch or an 
  attacker has been plugged in. BPDU Guard instantly disables and shuts down 
  the port (putting it into 'err-disable' state) to preserve network integrity.

* Root Guard (Protecting the Throne):
  Unlike BPDU Guard, Root Guard is applied strictly to core infrastructure Trunk 
  links facing downstream branch/untrusted switches. It permits standard BPDU 
  exchanges, but if a downstream switch tries to broadcast a BPDU claiming to 
  be the new Root Bridge, Root Guard immediately blocks the port and moves it 
  into a temporary 'root-inconsistent' state until the threat stops.

--------------------------------------------------------------------------------
3. REAL-WORLD TROUBLESHOOTING
--------------------------------------------------------------------------------

During this lab, I encountered and resolved a brilliant engineering scenario 
showing exactly how the switch behaves under different security environments.

### SCENARIO A: The BPDU Guard Trap & Interface Collapse
* The Environment: 
  I globally activated the automated protection network using the commands:
  `spanning-tree portfast default`
  `spanning-tree portfast bpduguard default`
  This meant Switch2 treated every single interface as an access-layer user port.
  
* The Action: 
  I plugged a cable from Switch2 into Switch3, while Switch3 was pre-configured 
  with a manipulative priority of 0 (trying to steal the Root throne).
  
* The Result (Image_9a4a7a.png): 
  The interface immediately collapsed! The switch generated this security error:
  "%SPANTREE-2-BLOCK_BPDUGUARD: Received BPDU on port FastEthernet0/3... Disabling port."
  "%ERR_DISABLE: bpduguard error detected on 0/3, putting 0/3 in err-disable state."
  
* Why it Happened: 
  Because the switch assumed the link was for a standard computer. When it received 
  a strong BPDU from Switch3, the bodyguard (BPDU Guard) fired instantly and killed 
  the port to save the network, lighting up the link in solid red.
  
* The Recovery Lesson: 
  Typing `no shutdown` alone fails completely here. To recover an interface killed 
  by BPDU Guard, you must execute a proper sequence: Apply `shutdown` first to 
  clear the security error flag from the switch's memory, and then apply `no shutdown`.

### SCENARIO B: Proactive Strategy & Root Guard Success
* The Environment & Action (Image_9a5929.png): 
  Before establishing the link or after clearing the state, I properly modified the 
  port type. I manually declared the link as a Trunk and activated Root Guard:
  `switchport mode trunk`
  `spanning-tree guard root`
  
* The Result: 
  This time, the interface stayed up and operating successfully! The switch triggered 
  this smart alert:
  "%SPANTREE-2-ROOTGUARDBLOCK: Port 0/4 tried to become non-designated in VLAN 1."
  "Moved to root-inconsistent state"
  
* Why it Worked (The Reason):
  1. Turning the link into a Trunk informed the switch that a legitimate neighbor 
     infrastructure device is connected. Thus, it automatically deactivated PortFast 
     and BPDU Guard on this port, preventing the interface from crashing into 'err-disable'.
  2. When Switch3 sent its rogue BPDU trying to become the King, the armed Root Guard 
     captured it. Instead of killing the line, it isolated it into a 'root-inconsistent' 
     state. It completely blocks malicious traffic while leaving the port open to listen. 
     If Switch3 stops acting maliciously, the port automatically heals itself!
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
7. THE GOLDEN RULE OF ROOT GUARD: WHERE AND WHY TO DEPLOY IT
--------------------------------------------------------------------------------

A major architectural mistake many beginners make is thinking: 
"To secure my network, I should just enable Root Guard on ALL trunk ports between 
all switches!"

### Why is global Root Guard a dangerous mistake?
Spanning Tree Protocol (STP) relies on "Redundancy" (backup paths). If your primary 
Root Bridge (the King Switch) goes down due to a power outage or maintenance, the 
other core switches must immediately negotiate and elect a secondary backup Root Bridge 
to keep the company's network alive. 

If you put Root Guard on the trunks between your core switches, they will block each other 
from becoming the new backup King! The interfaces will go into a "root-inconsistent" 
state, the core links will fail, and the entire enterprise network will crash.

### The Golden Engineering Rule:
> **"Root Guard must ONLY be deployed on downstream trunk ports leading to branch switches, 
> access switches, or untrusted network zones where a switch is NEVER allowed to become 
> the Root Bridge under any circumstance."**

### Applying the Golden Rule to our Topology:
Look at our network structure as a hierarchy:
1. Core Layer (Switch1, Switch0, Switch2): 
   These are our main high-level switches. We NEVER put Root Guard on the links 
   between them, because they need the freedom to hold emergency elections if Switch1 fails.

2. Branch/Untrusted Layer (Switch3): 
   Switch3 is located downstream (at a branch or a student lab). We do NOT trust it, and 
   it must never become the manager of our network.

3. The Perfect Placement:
   We activate Root Guard on the specific port inside **Switch2** that points directly 
   DOWN toward **Switch3**. 
   * Meaning: *"Hey Switch2, you can talk to Switch3 normally. But if Switch3 suddenly sends 
     a BPDU packet saying 'I am the new King', smack it down immediately and block that port 
     to protect the core hierarchy!"*

###  Summary:
* Ports facing UP toward the legitimate King $\rightarrow$ **Standard Trunk (No Root Guard).**
* Ports facing DOWN toward untrusted/branch switches $\rightarrow$ **Trunk + Root Guard.**
================================================================================
4. STEP-BY-STEP COMMAND LINE INTERFACE (CLI) IMPLEMENTATION
--------------------------------------------------------------------------------

STEP 1: SECURING THE ROOT BRIDGE ARRANGEMENT (THE KING)
------------------------------------------------------
Instead of relying on random calculations, we manually lock down the priority 
of our core switch (Switch1) to the absolute lowest value (0) to ensure its 
permanent status as the legitimate Root Bridge.

Switch1(config)# spanning-tree vlan 1 priority 0

STEP 2: ENABLING AUTOMATED GLOBAL PROTECTION ON ACCESS SWITCHES (SW0 & SW1)
---------------------------------------------------------------------------
Rather than entering every single port manually, we apply an automated, 
enterprise-wide security strategy. Any port configured as an access link 
will inherit PortFast and BPDU Guard instantly.

Switch0(config)# spanning-tree portfast default

Switch0(config)# spanning-tree portfast bpduguard default


STEP 3: HARDENING TRANSLATING TRUNK LINKS & ENFORCING ROOT GUARD (SW2)
----------------------------------------------------------------------
When preparing a link toward an untrusted branch expansion switch (Switch3), 
we declare the link as a Trunk, which safely bypasses the access-layer BPDU Guard, 
and we immediately arm the Root Guard to protect our core network.

Switch2(config)# interface fastEthernet 0/4

Switch2(config-if)# switchport mode trunk

Switch2(config-if)# spanning-tree guard root

--------------------------------------------------------------------------------
5. VERIFICATION COMMANDS & EXPECTED OUTPUTS
--------------------------------------------------------------------------------

Command 1: Checking Global Security Status

Switch0# show spanning-tree summary

-> Verify that PortFast Default and BPDU Guard Default are listed as Enabled.

Command 2: Verifying Root Guard Enforcement

Switch2# show spanning-tree interface f0/4 details

-> When Switch3 tries to claim the Root status, Switch2 will cleanly capture it. 
   The interface will automatically appear here under 'Root Inconsistent' state, 
   proving our defense architecture is active and successful!

--------------------------------------------------------------------------------
6. COMMAND PRECEDENCE: GLOBAL DEFAULTS VS. PORT-LEVEL CONFIGURATION
--------------------------------------------------------------------------------

A critical question every network learner asks is: 
"What is the exact difference between these two execution methods, and what happens 
if I configure both on the same switch?"

Method 1: Global Automation (The "Default" Way)
----------------------------------------------
Switch0(config)# spanning-tree portfast default

Switch0(config)# spanning-tree portfast bpduguard default

Method 2: Manual Enforcement (The "Per-Interface" Way)
------------------------------------------------------
Switch0(config-if)# spanning-tree portfast 

Switch0(config-if)# spanning-tree bpduguard enable

### The Golden Engineering Rule: "Specific Overrides Global"
In Cisco IOS architecture, we use a concept called "Command Precedence". Think of the 
switch as a smart judge who manages two books: 
1. The "Global Constitution Book" (The Global Default settings).
2. The "Special Direct Orders Book" (The manual settings you type inside the interface).

Whenever a conflict occurs, the switch will ALWAYS choose the "Special Direct Orders" 
over the "Global Constitution". The specific port configuration completely overrides 
the global default.

### Detailed Scenario: How the Switch Thinks
Imagine you enter ports `Fa0/1` through `Fa0/10` using the `interface range` command 
and configure them manually. Later, you exit to global configuration mode and turn on 
the global `default` commands. Here is exactly how the switch divides its 24 ports:

1. Ports Configured Manually (Fa0/1 to Fa0/10):
   These ports will strictly follow the direct manual orders you injected inside the 
   range. The switch will completely ignore the global "default" commands for these 
   10 ports because you gave them a clear, explicit, specific command.

2. Unused or Remaining Ports (Fa0/11 to Fa0/24):
   Since you did not type anything manually inside these interfaces, the smart switch 
   steps in and applies the "Global Constitution" (the defaults) to them. The moment 
   any of these remaining ports becomes an Access port, the switch automatically arms 
   PortFast and BPDU Guard on them as a safety net!

### Scenario Simulation: An Interesting Conflict
What if we configure a purposeful contradiction?

Step 1: You enter interface `Fa0/5` and manually disable the bodyguard:

Switch0(config-if)# spanning-tree bpduguard disable

Step 2: You go back to global mode and turn on the default bodyguard for the whole switch:

Switch0(config)# spanning-tree portfast bpduguard default

How does the switch process this?

The switch will say: *"The Global Constitution tells me that BPDU Guard must be active 
on all Access ports. However, my engineer personally walked into interface Fa0/5 and 
explicitly told me to 'disable' it. Therefore, I will respect the specific order and leave 
Fa0/5 unprotected, while I will strictly enforce the law and protect all other ports from 
6 to 24!"*

### Learner's Summary:

You can mix both configuration methods without any worries! Global defaults act as a 
broad safety net protecting any interface you might forget or skip, while your explicit 
interface commands always hold the ultimate authority on the ports you select manually.
