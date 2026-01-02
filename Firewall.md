# Firewall Setup in Kali Linux (macOS M2)

## Overview
This project documents the setup and configuration of a firewall in **Kali Linux** using **UFW (Uncomplicated Firewall)**, running on **macOS (Apple M2)**.  
The goal is to practice firewall rule configuration, secure VM traffic, and simulate real-world defense scenarios.

---

## Environment
- **Host OS:** macOS (Apple M2)
- **Virtualization Tool:** VirtualBox 
- **Guest OS:** Kali Linux  
- **Network Mode:** Bridged  

---

## Firewall Tool Used
- **Tool:** UFW (Uncomplicated Firewall)  
- **Default Policy:** Deny all incoming, allow all outgoing  

---


## Installation
Before configuring rules, install UFW:
```bash
sudo apt update
sudo apt install ufw -y
```

## Rules Configured

### 1. Enable UFW
```bash
sudo ufw enable
```

### 2. Set Default Policies
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### 3. Add Rules
```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw deny proto icmp from any to any
```

- Allow SSH (port 22), HTTP/HTTPS (port 80 & 443)
- Block ICMP (ping requests)

### 4. Display Rules
```bash
sudo ufw status
sudo ufw status numbered
```

### 5. Delete Rules
- Delete by rule number (from ufw status numbered):
```bash
sudo ufw delete 2
```
- Delete by retyping the rule:
```bash
sudo ufw delete allow 80/tcp
```
![Firewall Rules](./ufwrule.png)
## Testing
- **Ping Test**: Blocked successfully
```bash
ping <vm_ip>
```
- **HTTP test**: Allowed
```bash
curl http://<vm_ip>
```
- **SSH test (port 22)**: Allowed:
```bash
ssh user@<vm_ip>
```
![DoS](./ufwdos.jpeg)

-**nmap scan**: Confirmed restricted ports
```bash
namp <vm_ip>
```
## Conclusion

The firewall setup in Kali Linux using UFW on macOS (Apple M2) was successfully implemented and tested.
All configured rules — including SSH (22), HTTP (80), HTTPS (443), and ICMP blocking — worked as expected.
Testing with ping, curl, ssh, and nmap confirmed that the firewall rules were effective, proving the setup to be secure and reliable.
This project demonstrates not only the practical application of UFW but also the importance of proactive defense in cybersecurity.
