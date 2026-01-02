# Snort 3.9 IPS Setup in Kali Linux (macOS M2)

## Overview
This project documents the installation and configuration of **Snort 3.9**, an open-source Intrusion Prevention System (IPS), in **Kali Linux** running on **macOS (Apple M2)**.  
Unlike IDS mode, IPS actively blocks malicious traffic inline, enforcing security policies in real time.

---

## Environment
- **Host OS:** macOS (Apple M2)
- **Virtualization Tool:** VirtualBox 
- **Guest OS:** Kali Linux 
- **Network Mode:** Bridged  

---
## Prerequisites

Since Snort is already installed (IDS setup done), add iptables for IPS mode:

```bash
sudo apt update
sudo apt install iptables
```
## Configuration (IPS mode)

### 1. Local Rules
```bash
sudo nano /etc/snort/rules/local.rules
drop icmp any any -> any any (msg: "ICMP Packet Dropped"; sid:2000001; rev:1;)
```
![IPS rules](./ipsrules.png)

### 2.  Include Rules in `snort.lua`
```bash
sudo nano /etc/snort/snort.lua
include = '/etc/snort/rules/local.rules'
```
## Running Snort in IPS mode
### 1. Redirect Traffic to NFQUEUE
```bash
sudo iptables -I INPUT -j NFQUEUE
sudo iptables -I OUTPUT -j NFQUEUE
```
### 2. Start Snort in IPS Mode
```bash
sudo snort -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules -Q --daq nfq -i eth0
```
![IPS](./ips.png)

## Testing 
- **Ping test**: Dropped successfully
  ```bash
  ping <vm_ip>
  ```
- **HTTP Test**: Allowed
  ```bash
  curl http://<vm_ip>
  ```
- **SSH Test**: Allowed
  ```bash
  ssh user@<vm_ip>
  ```
  ![IPS DoS Attack](./ipsdos.png)

## Managing Rules
### 1. Add New Rule
```bash
sudo nano /etc/snort/rules/local.rules
```
### 2. Stop and restart Snort to apply new rules:
```bash
sudo snort -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules -Q --daq nfq -i eth0
```
## Conclusion
The Snort 3.9 IPS setup in Kali Linux on macOS (Apple M2) was successfully implemented and tested.
Snort actively blocked malicious traffic (ICMP) while allowing legitimate services (HTTP, SSH), proving its effectiveness as an Intrusion Prevention System.
This project demonstrates hands-on experience in Snort IPS mode, proactive defense, and inline traffic control.
  
