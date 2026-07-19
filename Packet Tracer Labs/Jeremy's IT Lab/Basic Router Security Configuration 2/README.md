# Lab 002: Basic Router Security Configuration 2

## 📌 Topology & Scenario Overview
This lab demonstrates the differences between `enable password` and `enable secret` in Cisco IOS. You will learn about password precedence (which password takes priority) and how each command stores passwords in the device configuration memory.

---

## 🛠️ Step-by-Step Configuration & Technical Deep-Dive

### Step 1 & 2: Physical Topology & Device Identity
1. Connect **R1** and **R2** from their `GigabitEthernet0/0` interfaces using a **Crossover Cable** (since they are similar devices).
2. Configure the hostnames:
   * **R1:** `hostname R1`
   * **R2:** `hostname R2`

### Step 3 & 4: Configuring Both Passwords
Apply both types of enable passwords on the routers.
* **Commands (Apply on both R1 & R2):**
  ```text
  R1(config)# enable password cisco
  R1(config)# enable secret ccna
  ```
