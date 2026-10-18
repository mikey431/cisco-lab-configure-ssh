# Cisco Packet Tracer Lab: Configure Network Devices with SSH

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-049FD9?style=for-the-badge&logo=cisco&logoColor=white)
![Network Security](https://img.shields.io/badge/Security-SSH-red?style=for-the-badge)

## 📌 Overview
This repository contains a Cisco Packet Tracer laboratory focused on configuring **Secure Shell (SSH)** on Cisco routers and switches. 

Telnet transmits data, including passwords, in plain text, making it vulnerable to network sniffing. **SSH** provides an encrypted management connection, ensuring confidentiality and authentication for remote administration.

---

## 📐 Network Topology & Addressing Table

```
       +--------------+            +--------------+            +--------------+
       |     PC-A     |------------|   Switch S1  |------------|   Router R1  |
       +--------------+            +--------------+            +--------------+
```

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **R1** | G0/0/1 | `192.168.1.1` | `255.255.255.0` | N/A |
| **S1** | VLAN 1 | `192.168.1.11` | `255.255.255.0` | `192.168.1.1` |
| **PC-A**| NIC | `192.168.1.3` | `255.255.255.0` | `192.168.1.1` |

---

## 🎯 Lab Objectives

1. **Part 1:** Configure Basic Device Settings (Router, Switch, PC).
2. **Part 2:** Configure Router R1 for SSH Access.
3. **Part 3:** Configure Switch S1 for SSH Access.
4. **Part 4:** Verify SSH Connectivity via CLI/Terminal Emulation.

---

## 🛠️ Required Resources

* **1 Router:** Cisco 4221 (Cisco IOS XE Release 16.9.4 or equivalent)
* **1 Switch:** Cisco Catalyst 2960 (Cisco IOS Release 15.2(2) or equivalent)
* **1 PC:** Windows PC with Terminal Emulation Software (e.g., Tera Term, Packet Tracer PC Command Prompt)
* Ethernet and Console Cables

---

## 🚀 Step-by-Step Configuration

### Part 1: Basic Device Configuration

#### **Router (R1)**
```cisco
enable
configure terminal
hostname R1
no ip domain-lookup
enable secret class
line console 0
 password cisco
 login
 exit
line vty 0 4
 password cisco
 login
 exit
service password-encryption
banner motd # Unauthorized access is strictly prohibited! #
interface GigabitEthernet0/0/1
 ip address 192.168.1.1 255.255.255.0
 no shutdown
 exit
end
copy running-config startup-config
```

#### **Switch (S1)**
```cisco
enable
configure terminal
hostname S1
no ip domain-lookup
enable secret class
line console 0
 password cisco
 login
 exit
line vty 0 15
 password cisco
 login
 exit
service password-encryption
banner motd # Unauthorized access is strictly prohibited! #
interface vlan 1
 ip address 192.168.1.11 255.255.255.0
 no shutdown
 exit
ip default-gateway 192.168.1.1
end
copy running-config startup-config
```

---

### Part 2: Configure SSH on Router (R1)

```cisco
configure terminal
ip domain-name netacad.pka
crypto key generate rsa
# Enter 1024 (or higher) for key length
username admin secret Adm1nP@55
line vty 0 4
 transport input ssh telnet
 login local
 exit
end
copy running-config startup-config
```

---

### Part 3: Configure SSH on Switch (S1)

```cisco
configure terminal
ip domain-name netacad.pka
crypto key generate rsa
# Enter 1024 (or higher) for key length
username admin secret Adm1nP@55
line vty 0 15
 transport input ssh telnet
 login local
 exit
end
copy running-config startup-config
```

---

## ✅ Verification & Testing

1. **Ping Test:** Verify basic IP reachability from `PC-A` to `R1` (`192.168.1.1`) and `S1` (`192.168.1.11`).
2. **SSH Connection:** From PC-A terminal / Command Prompt, initiate SSH sessions:
   ```bash
   ssh -l admin 192.168.1.1   # For Router
   ssh -l admin 192.168.1.11  # For Switch
   ```
3. Enter password: `Adm1nP@55` when prompted.

---

## 📄 License & Credits
* Based on Cisco Networking Academy (NetAcad) CCNA curriculum.
* Created and tested in Cisco Packet Tracer.