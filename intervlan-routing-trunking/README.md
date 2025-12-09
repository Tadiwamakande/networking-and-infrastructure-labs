# Inter-VLAN Routing & Trunking Configuration Project (Packet Tracer)

## Overview

This project demonstrates the implementation of **inter-VLAN routing** using a router-on-a-stick design. The tasks involve creating VLANs, assigning switch ports, configuring a static trunk, building router subinterfaces for VLAN routing, and validating end-to-end connectivity across all hosts and the server.

The goal is to segment the network into logical broadcast domains while enabling Layer 3 communication between them through properly configured subinterfaces on R1.

---

## Objectives

- Assign IPv4 addresses to R1 and S1 according to the Addressing Table.  
- Configure the default gateway on S1.  
- Create, name, and assign VLANs on S1 exactly as specified in the VLAN and Port Assignments Table.  
- Set all access ports to the appropriate VLAN.  
- Configure interface **G0/1 on S1 as a static trunk** and assign the native VLAN.  
- Disable all switch ports not assigned to a VLAN.  
- Configure inter-VLAN routing on R1 using subinterfaces.  
- Verify full connectivity between R1, S1, end hosts, and the server.

---

## Skills Demonstrated

- VLAN creation and port assignment  
- Access and trunk port configuration  
- Native VLAN configuration  
- Router-on-a-stick inter-VLAN routing  
- Subinterface encapsulation using `dot1Q`  
- Switch security practices (disabling unused ports)  
- End-to-end connectivity testing across VLANs  

---

## Key Configuration Elements

### VLAN Configuration (S1)

- Create and name VLANs according to the VLAN table  
- Assign ports to correct VLANs in **access mode**  
- Disable unused ports:  
interface range Fa0/2 – 24
shutdown


### Trunk Configuration (S1)

interface GigabitEthernet0/1
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk native vlan <native_vlan_id>


This trunk carries traffic for all created VLANs to the router.

---

## Inter-VLAN Routing (R1)

Subinterfaces are created on R1’s G0/0 interface:

interface GigabitEthernet0/0.<vlan_id>
encapsulation dot1Q <vlan_id> [native]
ip address <gateway_ip> <subnet_mask>

vbnet


Each subinterface serves as the default gateway for its VLAN.

Example:
interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0


---

## Switch Default Gateway (S1)

ip default-gateway <router_vlan_interface_ip>


This allows S1 to reach networks beyond its own VLAN.

---

## Testing & Verification

After configuration:

### Expected Outcomes  
✔ PCs in different VLANs can ping each other  
✔ All hosts can reach the server  
✔ S1 and R1 can ping all VLAN interfaces  
✔ Trunk and subinterfaces show up/up  

### Useful Commands

**On S1:**
show vlan brief
show interfaces trunk
show running-config


**On R1:**
show ip interface brief
show running-config | section interface
ping <host_ip>


---

## Key Learnings

- How VLANs segment a Layer 2 network into multiple broadcast domains  
- How trunk ports transport multiple VLANs over a single link  
- The role of native VLANs in 802.1Q encapsulation  
- How router-on-a-stick enables Layer 3 communication between VLANs  
- Network hygiene practices: shutting down unused switch ports  

---

## How to Use This Lab

1. Open the `.pkt` file in Cisco Packet Tracer 8.2 or later.  
2. Check VLAN creation and port assignments on S1.  
3. Verify trunk configuration on G0/1:  
show interfaces trunk
4. Inspect router subinterfaces:  
show running-config | section g0/0
5. Test connectivity across all VLANs and to the server:  
ping <server_ip>




---

## Repository Structure

intervlan-routing-trunking/
├── vlan-lab.pkt # Packet Tracer lab file
├── topology.png # Optional topology diagram
└── README.md # Project documentation (this file)
