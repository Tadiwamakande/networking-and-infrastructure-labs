# Enterprise Network Simulation Project (Packet Tracer)

## Overview

This project simulates a small enterprise network involving multiple departments, routed subnets, VLANs, and basic device security hardening. The goal was to design a new IPv4 addressing scheme, configure IPv6 addressing, secure router and switch access, and verify full connectivity both internally and to hosted web services.

The simulation uses the `192.168.0.0/24` network and requires subnetting to support Staff, Sales, IT, and a future Guest network—each with different host requirements.

---

## Objectives

- Create a scalable IPv4 addressing scheme for four enterprise subnets.  
- Assign IPv6 addressing and default gateways.  
- Configure R1 with secure management settings (SSH, password policies, banners).  
- Configure SVI interfaces and security settings on switches S1, S2, and S3.  
- Verify that all devices can reach internal services and web servers (`www.cisco.srv` and `www.cisco6.srv`).  

---

## Subnetting Requirements

Using the **192.168.0.0/24** network:

| Department | Hosts Needed | Subnet Size | Purpose |
|-----------|--------------|-------------|---------|
| Staff     | 100          | /25         | Largest subnet |
| Sales     | 50           | /26         | Mid-size subnet |
| IT        | 25           | /27         | Small subnet |
| Guest     | 25 (future)  | /27         | Reserved for future expansion |

You must document IPv4 assignments in the Addressing Table and record the Guest subnet even if it is unused for now.

---

## Device Configuration Summary

### PC Configurations

- Assign IPv4 address, subnet mask, and default gateway based on the created addressing scheme.  
- Assign IPv6 unicast and link-local addresses based on the Addressing Table.  
- Verify connectivity between departments and external services.

---

## Router R1 Configuration

### General Settings
- Set device hostname according to Addressing Table.  
- Disable DNS lookup:  
no ip domain-lookup

- Set encrypted privileged EXEC password (`Ciscoenpa55`).  
- Set console password (`Ciscoconpa55`) and enable login.  
- Enforce minimum password length of **10 characters**.  
- Encrypt all plaintext passwords:  
service password-encryption
- Add security banner restricting unauthorized access.

---

### Interface Configuration
Configure all Gigabit Ethernet interfaces with:

- Correct IPv4 address and subnet mask  
- Correct IPv6 address and default IPv6 gateway  
- Administrative `no shutdown`

---

### SSH Configuration
- Set domain name:  
ip domain-name CCNA-lab.com

- Generate **1024-bit RSA key**:  
crypto key generate rsa general-keys modulus 1024 diff
- Configure VTY lines for SSH access only.  
- Create admin user:  
username Admin1 privilege 15 secret Admin1pa55


---

### Additional Security
- Auto-logout after 5 minutes of inactivity on console and VTY lines.  
- Enable login block-for policy:  
- Block for **3 minutes**  
- After **4 failed attempts**  
- Within **2 minutes**

Example:
login block-for 180 attempts 4 within 120




---

## Switch Configuration (S1, S2, S3)

- Set hostname according to Addressing Table.  
- Disable DNS lookup.  
- Configure SVI interface with IPv4 address and subnet mask.  
- Set default gateway to router’s VLAN interface.  
- Configure console and VTY passwords.  
- Apply inactivity timer (5 minutes).  
- Encrypt all plaintext passwords.  

Each switch forms the Layer 2 boundary for Staff, Sales, and IT networks.

---

## Connectivity Requirements

All hosts must:

- Successfully browse to `www.cisco.srv`  
- Successfully browse to `www.cisco6.srv`  
- Ping all other IPv4 and IPv6 devices in the network  
- Reach default gateways and R1 router  

This confirms both addressing and security configurations are correct.

---

## Skills Demonstrated

- Enterprise subnet planning and VLSM design  
- Deployment of dual-stack IPv4/IPv6 addressing  
- Router and switch security hardening  
- SSH-based remote management configuration  
- Logical interface configuration including SVIs  
- End-to-end connectivity verification  
- Understanding of hierarchical enterprise network design  

---

## How to Use This Lab

1. Open `enterprise-network.pkt` in Cisco Packet Tracer 8.2+.  
2. Review subnetting assignments in the Addressing Table.  
3. Verify R1 security configuration:  
show running-config | section line
show ip ssh
4. Test connectivity using:  
ping 192.168.x.x
ping ipv6-address
5. Use PC web browsers to reach:  
- `www.cisco.srv`  
- `www.cisco6.srv`  

---

## Repository Structure

enterprise-network-simulation/
├── enterprise-network.pkt # Packet Tracer file
├── router-configs/ # Optional saved configs
│ ├── R1.txt
│ ├── S1.txt
│ ├── S2.txt
│ └── S3.txt
├── topology.png # Optional topology diagram
└── README.md # Documentation (this file)
