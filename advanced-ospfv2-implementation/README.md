# Advanced OSPFv2 Implementation (Packet Tracer)

## Overview

This project implements a full single-area **OSPFv2** configuration across a mixed topology containing both **point-to-point serial links** and **broadcast multiaccess networks**. The goal was to complete the OSPF setup according to a set of engineering requirements, verify neighbour adjacencies, tune OSPF behaviour, and ensure full network reachability — including successful connectivity to an external internet server.

---

## Objectives

- Implement OSPFv2 across all routers using **process ID 10**.  
- Configure OSPF on both point-to-point and multiaccess segments.  
- Assign required **router IDs** on multiaccess routers.  
- Adjust interface priorities to enforce a specific **Designated Router (DR)**.  
- Tune OSPF interface costs (Gigabit, FastEthernet, and a specific Serial link).  
- Prevent unnecessary routing updates using passive interfaces.  
- Create and redistribute a **default route** toward the ISP.  
- Modify OSPF timers on specific interfaces.  

All hosts in the network should be able to **ping the external internet server** once configuration is complete.

---

## Topology Summary

The lab includes:

- **Headquarters routers** (point-to-point links)  
- **Data Service routers** (LANs with multiple hosts)  
- **Multiaccess broadcast segment** with routers BC-1, BC-2, BC-3  
- **ISP cloud** providing external connectivity  

(Optional: Add `topology.png` for visual reference.)

---

## Skills Demonstrated

- Multi-router OSPFv2 configuration  
- Router ID planning and manual assignment  
- DR/BDR election control using interface priority  
- OSPF network statements and wildcard masks  
- OSPF interface-based activation (Data Service network)  
- Default route creation and redistribution  
- OSPF cost tuning and path influence  
- Hello/dead timer modification  
- Ensuring full network convergence in a mixed topology  

---

## Key Configuration Elements

### OSPF Activation (Process ID 10)
- OSPF enabled globally on all routers  
- Headquarters network uses **network statements + wildcard masks**  
- Data Service routers use **interface-level OSPF activation**  

---

### Router ID Assignments (Multiaccess Routers)

- **BC-1 → 6.6.6.6**  
- **BC-2 → 5.5.5.5**  
- **BC-3 → 4.4.4.4**

Router IDs ensure predictable neighbour relationships and control over DR/BDR roles.

---

### DR/BDR Control

BC-1 must always become the **Designated Router (DR)** on the broadcast network by configuring the **highest OSPF interface priority**:



ip ospf priority <highest_value>


---

### Preventing Unnecessary OSPF Updates

Non-transit interfaces (e.g., LANs with end devices) were configured as passive:



passive-interface GigabitEthernet0/0


This prevents OSPF hello packets from being sent where no neighbours exist.

---

### Default Route & Redistribution

A default route is created toward the ISP using the **exit-interface** form:



ip route 0.0.0.0 0.0.0.0 Serial0/x/x


Then redistributed into OSPF:



default-information originate


All routers should learn this default route for internet access.

---

### OSPF Cost Adjustments

- GigabitEthernet interfaces set to cost **10**  
- FastEthernet interfaces set to cost **100**  
- Specific Serial interface (`P2P-1 Serial0/1/1`) set to cost **50**

Example:



ip ospf cost 10


These cost settings ensure correct path selection based on link speed.

---

### Timer Modifications

On interfaces connecting **P2P-1 and BC-1**, OSPF hello and dead timers were doubled:



ip ospf hello-interval <value>
ip ospf dead-interval <value>


This ensures stable neighbour relationships.

---

## Testing & Verification

### Expected Outcomes

All routers form full OSPF adjacencies  
BC-1 elected as DR  
All networks appear in each router’s routing table  
Default route propagated from ISP side  
All hosts can ping the internet server  

### Useful Verification Commands



show ip ospf neighbor
show ip ospf interface
show ip ospf database
show ip route ospf
show running-config | section ospf


---

## Key Learnings

- How OSPF behaves differently on broadcast vs point-to-point networks  
- The importance of manual router IDs for predictable network design  
- How DR/BDR elections are influenced by interface priority  
- Link cost tuning to influence routing decisions  
- Proper use of passive interfaces to optimize OSPF operation  
- Default route redistribution and its impact on end-to-end connectivity  
- Timer tuning for stability in sensitive link environments  

---

## How to Use This Lab

1. Open the `.pkt` file in **Cisco Packet Tracer 8.2 or later**.  
2. Review OSPF configuration on each router:  


show running-config | section ospf

3. Verify OSPF adjacencies:  


show ip ospf neighbor

4. Test routing tables and default route distribution.  
5. Ping the provided internet server from various LAN hosts.  

---

## Repository Structure



advanced-ospfv2-implementation/
├── ospf-lab.pkt # Packet Tracer lab file
├── topology.png # Optional topology diagram
└── README.md # Project documentation (this file)
