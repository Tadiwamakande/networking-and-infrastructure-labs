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


