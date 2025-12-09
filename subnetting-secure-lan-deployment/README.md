# Subnetting & Secure LAN Deployment Project (Packet Tracer)

## Overview

This project focuses on designing a scalable IPv4 addressing scheme using **VLSM**, configuring IPv6 addressing, securing router and switch access, and validating full connectivity across an enterprise LAN. The environment includes Staff, Sales, IT, and Guest networks, each with different host requirements. After subnetting the `192.168.0.0/24` network, the router (R1) and switches (S1, S2, S3) were configured to support secure network operations and dual-stack communication.

---

## Objectives

- Create an IPv4 VLSM subnetting plan supporting four enterprise networks.  
- Configure IPv6 addressing on R1 and end devices.  
- Apply security hardening on router and switch management interfaces.  
- Configure SVIs and default gateways on all switches.  
- Ensure all PCs can access internal resources and hosted web servers.  

---

## Subnetting Requirements (VLSM)

Using **192.168.0.0/24**, the network must support:

| Department | Hosts Needed | Recommended Subnet | Notes |
|-----------|--------------|--------------------|-------|
| Staff     | 100          | /25                | Largest subnet |
| Sales     | 50           | /26                | Mid-size subnet |
| IT        | 25           | /27                | Small subnet |
| Guest     | 25 (future)  | /27                | Reserved |

You must:

- Assign subnets in the Addressing Table.  
- Record the Guest network subnet.  
- Assign IPv4 addresses, masks, and gateways based on this plan.

---

## PC Configurations

For **Staff, Sales, and IT PCs**:

- Configure IPv4 addresses, subnet masks, and default gateways.  
- Configure IPv6 unicast addresses, link-local addresses, and default gateways.  

Each PC must be able to reach:

- All other devices  
- The internal web server (`www.cisco.srv`)  
- The IPv6 web server (`www.cisco6.srv`)

---

## Router R1 Configuration

### General Security Settings
- Set hostname according to Addressing Table  
- Disable DNS lookup:  
no ip domain-lookup


- Set encrypted privileged EXEC password: `Ciscoenpa55`  
- Set console password: `Ciscoconpa55`  
- Enforce **minimum 10-character passwords**  
- Encrypt all plaintext passwords:  
service password-encryption
- Configure legal warning banner  

---

### Interface Configuration
Configure and enable all Gigabit Ethernet interfaces:

- Apply IPv4 addresses based on subnetting table  
- Apply IPv6 addresses according to addressing table  
- Bring interfaces up with `no shutdown`

---

### SSH Configuration
ip domain-name CCNA-lab.com
crypto key generate rsa modulus 1024
username Admin1 privilege 15 secret Admin1pa55
line vty 0 4
transport input ssh
login local

markdown
Copy code

### Additional Security
- Auto-logout after **5 minutes** on console and VTY lines  
- Enable login blocking:  
login block-for 180 attempts 4 within 120

---

## Switch Configuration (S1, S2, S3)

- Hostname configuration  
- Disable DNS lookup  
- Configure SVI interface with IPv4 address and subnet mask  
- Set default gateway pointing to R1  
- Configure console/VTY passwords  
- Encrypt all plaintext passwords  
- Apply inactivity timeout of 5 minutes  

Example SVI:
interface vlan 10
ip address 192.168.x.2 255.255.255.y
no shutdown

---

## Connectivity Requirements

All hosts must be able to:

- Access **www.cisco.srv** (IPv4 web server)  
- Access **www.cisco6.srv** (IPv6 web server)  
- Ping one another across VLANs  
- Ping R1 and all switch management interfaces  

This verifies IPv4 addressing, IPv6 addressing, routing, and switch gateway configurations.

---

## Skills Demonstrated

- VLSM subnetting and IPv4 address planning  
- Dual-stack IPv4/IPv6 end device configuration  
- Router and switch security hardening  
- SSH remote access configuration  
- SVI configuration and Layer 3 switch management  
- Understanding enterprise segmentation and multi-user networks  
- End-to-end connectivity validation  

---

## How to Use This Lab

1. Open the `.pkt` file in Cisco Packet Tracer 8.2+.  
2. Verify VLSM assignments in the Addressing Table.  
3. View router configuration:  
show running-config | begin hostname
4. Test IPv4 connectivity:  
ping 192.168.x.x
5. Test IPv6 connectivity:  
ping ipv6-address
6. Use PC web browsers to reach:  
- **http://www.cisco.srv**  
- **http://www.cisco6.srv**  

---

## Repository Structure

subnetting-secure-lan-deployment/
├── secure-lan.pkt # Packet Tracer lab file
├── topology.png # Optional diagram
└── README.md # Project documentation (this file)
