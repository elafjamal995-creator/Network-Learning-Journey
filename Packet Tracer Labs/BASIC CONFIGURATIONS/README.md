# Navigating Cisco IOS Modes

A reference guide for CLI mode hierarchy.

## The Hierarchy
| Level | Mode | Prompt | Capability |
| :--- | :--- | :--- | :--- |
| 1 | User EXEC | `Router>` | Basic diagnostics (ping, trace) |
| 2 | Privileged EXEC | `Router#` | View configs, save system state |
| 3 | Global Config | `Router(config)#` | Change global system settings |
| 4 | Interface/Line | `(config-if)#` | Configure specific hardware ports/lines |

## Navigation Shortcuts
* `exit`: Moves one step back in the hierarchy.
* `end` (or `Ctrl+Z`): Jumps immediately back to Privileged EXEC (`#`).
* `disable`: Jumps back to User EXEC (`>`).

## Engineering Tip
Always verify your prompt before typing a command. A command like `ip address` will only work in `(config-if)#`, not in the Global Config mode.

# Cisco Router/Switch Configuration Guide

This guide provides a comprehensive step-by-step procedure to secure and configure a Cisco device using the Command Line Interface (CLI).

## 1. Initial Device Setup
* **Set Hostname:** Change the device name for easy identification.
    * `configure terminal`
    * `hostname Router1`
* **Set Current Date/Time:** (Perform in Privileged EXEC mode `#`)
    * `clock set 22:05:00 12 July 2026`

## 2. Security & Access Control
* **Enable Secret Password:** Protects the Privileged EXEC mode with strong encryption.
    * `enable secret MyStrongPassword`
* **Console Access:** Password protect the physical console port.
    * `line con 0` -> `password MyConsolePass` -> `login`
* **Remote Access (VTY):** Protect Telnet/SSH access (sessions 0-4).
    * `line vty 0 4` -> `password MyTelnetPass` -> `login`
* **User Management:** Create a local admin user with full privileges.
    * `username admin privilege 15 secret AdminPass`
* **Login Brute Force Protection:** Block logins if a user fails 3 times within 30 seconds.
    * `login block-for 60 attempts 3 within 30`

## 3. System Hardening & Maintenance
* **Encrypt All Passwords:** Automatically encrypts plain-text passwords in the configuration file.
    * `service password-encryption`
* **Set Domain Name:** Required for SSH and encryption keys.
    * `ip domain-name mycompany.com`
* **Disable IP Domain Lookup:** Prevents the device from freezing when a command is mistyped.
    * `no ip domain-lookup`
* **Session Management:**
    * **Exec Timeout:** Log out idle users after 5 minutes and 30 seconds.
        * `exec-timeout 5 30`
    * **Logging Synchronous:** Keep the console prompt clean during system messages.
        * `logging synchronous`

## 4. Communication
* **Banner MOTD:** Set a warning message for users logging in.
    * `banner motd # Unauthorized access forbidden! #`

## 5. Saving Configuration
* **View Configuration:** See the current running configuration.
    * `show running-config`
* **Save Configuration:** Copy the current changes to permanent storage (NVRAM).
    * `copy running-config startup-config` (or simply `write`)
