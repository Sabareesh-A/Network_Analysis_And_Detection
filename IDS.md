# Snort 3.9 IDS Setup in Kali Linux (Windows VM)

## Overview
This project documents the installation and configuration of **Snort 3.9**, an open-source Intrusion Detection System (IDS), in **Kali Linux** running inside a **Windows Virtual Machine**.  
The goal is to monitor network traffic, detect suspicious activity, and simulate real-world intrusion detection scenarios.

---

## Environment
- **Host OS:** Windows 11  
- **Virtualization Tool:** VirtualBox 
- **Guest OS:** Kali Linux 
- **Network Mode:** Bridged  

---

## Installation Steps

### 1. Update System
```bash
sudo apt update
```
### 2. Install Snort
```bash
sudo apt install snort
```
### 3. Verify Installation
```bash
snort -v
```
## Configuration (Snort 3.x)
Snort 3 uses Lua-based configuration (`snort.lua`) instead of the older `snort.conf`.

### 1. Locate Configuration File
```bash
cd /etc/snort/
ls
```
### 2. Create a Custom Rule File
```bash
sudo nano /etc/snort/rules/local.rules
alert icmp any any -> any any (msg: "ICMP Packet Detected"; sid:1000001; rev:1;)
```
![IDS rules](./idsrules.png)
### 3. Edit `snort.lua` to Include Local Rules
```bash
sudo nano /etc/snort/snort.lua
include = '/etc/snort/rules/local.rules'
```
## Running Snort 3.9
### Start Snort in IDS Mode
```bash
sudo snort -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules -i eth0 -A alert_fast
```
## Testing 
- **Ping test**: Should trigger alert
  ```bash
  ping <vm_ip>
  ```
  ![IDS ping test](./idsping.jpeg)
- **HTTP Test**: Allowed
  ```bash
  curl http://<vm_ip>
  ```
- **SSH Test**: Allowed
  ```bash
  ssh user@<vm_ip>
  ```
  ![IDS Dos Attack](./idsdos.png)
  
## Managing Rules
### 1. Add New Rule
```bash
sudo nano /etc/snort/rules/local.rules
```
### 2. Stop and restart Snort to apply new rules:
```bash
sudo snort -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules -i eth0 -A alert_fast
```
## Conclusion
The Snort 3.9 IDS setup in Kali Linux on a Windows VM was successfully implemented and tested.
Snort detected and logged suspicious traffic based on custom rules, proving its effectiveness as a modern intrusion detection system.
This project demonstrates hands-on experience in Snort 3.x, network security monitoring, and rule-based traffic analysis

  
