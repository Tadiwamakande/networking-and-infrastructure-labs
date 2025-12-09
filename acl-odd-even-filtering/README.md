# Dynamic ACL Traffic Filtering – Odd/Even IP Logic (Packet Tracer)

## Overview

This project implements a dynamic extended Access Control List (ACL) to control ICMP traffic between two subnets based on whether the source host IP address is odd or even. The goal was to design an ACL that:

- Allows ICMP echo requests from **even-numbered hosts** in the `192.168.0.0/24` network to reach hosts in the `10.0.0.0/24` network.
- Allows the corresponding ICMP echo replies back from `10.0.0.0/24` to those even-numbered hosts.
- Denies all other ICMP traffic from `192.168.0.0/24` to `10.0.0.0/24`.
- Leaves all non-ICMP IP traffic unaffected.

The underlying network uses EIGRP for routing between the two routers and is already fully converged; the focus of this project is purely on the ACL design and placement.

---

## Topology

The lab consists of two routers and two directly connected subnets:

- **Right router**
  - `FastEthernet0/0`: `192.168.0.1/24` (Right-PC subnet)
  - `Serial0/0/0`: `1.1.1.2/24` (WAN link to left router) :contentReference[oaicite:0]{index=0}

- **Left router**
  - `FastEthernet0/0`: `10.0.0.1/24` (Left-PC subnet)
  - `Serial0/0/0`: `1.1.1.1/24` with ACL applied inbound  
    `ip access-group ODD_EVEN_ICMP in` :contentReference[oaicite:1]{index=1}

Routing is handled by **EIGRP 123** on both routers. 

You can add a diagram screenshot here as `topology.png` if you like.

---

## Objectives

- Allow ICMP echo traffic **only** from even-numbered hosts in `192.168.0.0/24` to the `10.0.0.0/24` network.
- Allow the matching ICMP echo replies to return from `10.0.0.0/24` to those even-numbered hosts.
- Block ICMP from odd-numbered hosts in `192.168.0.0/24` toward `10.0.0.0/24`.
- Ensure the ACL works for *any* even/odd host address, not just a single hardcoded IP.
- Keep all other IP traffic between the subnets permitted.

---

## Skills Demonstrated

- Extended ACL design using wildcard masks
- Pattern matching for **even host addresses** in a /24 subnet
- Correct ACL placement and direction (inbound on WAN interface)
- Understanding of ICMP echo vs echo-reply flows
- Working with an EIGRP-routed topology in Packet Tracer
- Systematic testing of ACL behaviour

---

## ACL Design

The extended ACL is defined on the **left router** as follows: :contentReference[oaicite:3]{index=3}

```bash
ip access-list extended ODD_EVEN_ICMP
 permit icmp 192.168.0.0 0.0.0.254 10.0.0.0 0.0.0.255 echo
 permit icmp 10.0.0.0 0.0.0.255 192.168.0.0 0.0.0.254 echo-reply
 deny   icmp 192.168.0.0 0.0.0.255 10.0.0.0 0.0.0.255
 permit ip any any

---

## Key points

192.168.0.0 0.0.0.254
The wildcard 0.0.0.254 matches all even-numbered hosts in 192.168.0.0/24
(e.g. 192.168.0.2, .4, .6, …, .254).

First line:
permit icmp 192.168.0.0 0.0.0.254 10.0.0.0 0.0.0.255 echo
→ Allows ICMP echo from even hosts in 192.168.0.0/24 to any host in 10.0.0.0/24.

Second line:
permit icmp 10.0.0.0 0.0.0.255 192.168.0.0 0.0.0.254 echo-reply
→ Allows the echo-reply traffic back from any host in 10.0.0.0/24 to even hosts in 192.168.0.0/24.

Third line:
deny icmp 192.168.0.0 0.0.0.255 10.0.0.0 0.0.0.255
→ Denies any other ICMP flows from 192.168.0.0/24 to 10.0.0.0/24
(this catches odd-numbered hosts and any unmatched ICMP).

Final line:
permit ip any any
→ Ensures all non-ICMP IP traffic continues to pass normally.

The ACL is then applied inbound on the left router’s Serial0/0/0 interface:

interface Serial0/0/0
 ip address 1.1.1.1 255.255.255.0
 ip access-group ODD_EVEN_ICMP in


This placement ensures filtering is done as traffic arrives from the right router toward the 10.0.0.0/24 network. 

Left_running-config

Testing & Verification

To validate the ACL, different host IP addresses were assigned to the right-side PC (e.g. 192.168.0.10 vs 192.168.0.11) and ICMP tests were performed:

Even host address (e.g. 192.168.0.10)

Ping from 192.168.0.10 → 10.0.0.x succeeds (ICMP echo permitted).

Echo-reply from 10.0.0.x back to 192.168.0.10 succeeds.

show access-lists ODD_EVEN_ICMP shows matches on the permit statements.

Odd host address (e.g. 192.168.0.11)

❌ Ping from 192.168.0.11 → 10.0.0.x fails (ICMP denied).

show access-lists ODD_EVEN_ICMP shows matches on the deny statement.

Useful verification commands
show access-lists ODD_EVEN_ICMP
show ip interface Serial0/0/0
ping 10.0.0.1

---

**Key Learnings**

How wildcard masks can be used to match patterns (even-numbered hosts) rather than individual IPs.

The importance of ACL ordering (more specific permits before broader denies).

Selecting the correct interface and direction for ACL application to filter only the desired traffic path.

Separating control of ICMP from other IP traffic so normal routing remains unaffected.

---

**How to Use:**

Open the .pkt file in Cisco Packet Tracer 8.2 or later.
Inspect the ACL configuration on the left router (show running-config or show access-lists).
Change the IP address of the right-side PC to different odd/even values.
Use ping to test connectivity from 192.168.0.x → 10.0.0.x and observe ACL behaviour.
