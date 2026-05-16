# 🌐 Full Routing Protocols Topology — Multi-Domain Enterprise Network

> A comprehensive Cisco Packet Tracer simulation of an enterprise-grade multi-protocol network, integrating OSPF (Multi-Area), EIGRP, and RIPv2 with full redistribution, VLAN segmentation, centralized DHCP, and Layer 2 security hardening.

---

## 🖼️ Network Topology

![Topology](topology.png)

---

## 📌 Project Overview

This project simulates a real-world enterprise network environment built entirely in **Cisco Packet Tracer**. It demonstrates deep knowledge of routing protocols, inter-domain redistribution, VLAN design, and switching security — covering skills typically required in CCNA/CCNP-level deployments.

The topology spans **three routing domains** interconnected through **ASBR routers**, with **23+ routers**, **9+ managed switches**, and **22+ end devices** across logically separated network zones.

---

## 🗺️ Topology Breakdown

### Routing Domains

| Domain | Protocol | Area/Process | Notes |
|---|---|---|---|
| Core Backbone | OSPF Area 0 | Process 1 | Backbone area connecting all zones |
| Branch Zone | OSPF Area 1 | Process 1 | Multi-router spoke area |
| Legacy Zone | RIPv2 | — | Redistributed subnets enabled |
| Remote Zone | EIGRP | AS (custom) | Full mesh partial topology |

### Key Network Segments (Point-to-Point Links)

| Link | Subnet | Connected Routers |
|---|---|---|
| ASBR2 ↔ OSPF Area 1 | 10.0.1.40/30 | ASBR2 — Router3 |
| Router3 ↔ Router4 | 10.0.1.20/30 | Router3 — Router4 |
| Router4 ↔ ASBR1 | 10.0.1.12/30 | Router4 — ASBR1 |
| ASBR1 ↔ Router2 | 10.0.1.4/30 | ASBR1 — Router2 |
| ABR ↔ Router11 | 10.0.1.44/30 | ABR — Router11 |
| Router11 ↔ Router10 | 10.0.1.52/30 | Router11 — Router10 |
| ABR ↔ Router9 | 10.0.1.48/30 | ABR — Router9 |
| EIGRP Zone | 10.0.1.56–1.68/30 | Router13–Router15–Router16 |

---

## ⚙️ Features & Implementation Details

### 🔁 Routing Protocols & Redistribution

- **OSPF Multi-Area** (Area 0 + Area 1) with proper ABR and ASBR roles
- **EIGRP** deployed in the remote zone with partial mesh topology
- **RIPv2** with `redistribute rip subnets` for classless routing
- **Bidirectional redistribution** on both ASBR1 and ASBR2:
  - `redistribute ospf 1 metric 1` → into RIP
  - `redistribute rip` → into OSPF
  - `redistribute ospf to eigrp` and vice versa on ASBR2
- Route loop prevention considerations applied at redistribution boundaries

### 🏢 VLAN Design & Router-on-a-Stick

Every router connected to a managed switch uses **Router-on-a-Stick** (IEEE 802.1Q subinterfaces):

```
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```

- Each subinterface serves a dedicated VLAN
- VLANs span: Area 1 VLANs, Area 0 VLANs, EIGRP VLANs, RIP Network VLANs

### 📡 Centralized DHCP Server (Router-Based)

A single router acts as the **centralized DHCP server** for all VLANs across the topology:

- Separate DHCP pools per VLAN/subnet
- `ip helper-address` configured on each subinterface to relay DHCP requests
- Serves clients in: Area 1 VLANs, Area 0 VLANs, EIGRP zone VLANs

```
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
!
ip dhcp excluded-address 192.168.10.1 192.168.10.10
```

### 🔒 Layer 2 Security Hardening

All access-layer switches are hardened with:

| Feature | Configuration |
|---|---|
| **PortFast** | `spanning-tree portfast default` (global mode on all access ports) |
| **BPDUGuard** | `spanning-tree portfast bpduguard default` (global) |
| **Port Security** | Enabled on access ports — limits MAC addresses per port |
| **Trunking** | 802.1Q trunks on uplinks to routers and inter-switch links |
| **Access Ports** | Assigned to correct VLANs, no DTP negotiation |

---

## 🧪 Connectivity Verification

End-to-end reachability confirmed across all routing domains via ping and traceroute:

### Cross-Domain Ping Tests

```
Source: 17.0.0.16 (EIGRP Zone)

ping 8.0.0.16  → SUCCESS (RIPv2 Zone)   TTL=121  avg 12ms
ping 14.0.0.16 → SUCCESS (OSPF Area 0)  TTL=120  avg 14ms
```

### Traceroute Analysis

**EIGRP → RIPv2 (9.0.0.16)** — 8 hops:
```
17.0.0.1 → 10.0.1.61 → 10.0.1.57 → 10.0.1.41 → 10.0.1.21 → 10.0.1.14 → 10.0.1.9 → 9.0.0.16
```
Path: EIGRP Zone → ASBR2 → OSPF Area 1 → ASBR1 → RIPv2 Zone ✅

**EIGRP → OSPF Area 0 (13.0.0.16)** — 9 hops:
```
17.0.0.1 → 10.0.1.61 → 10.0.1.57 → 10.0.1.41 → 10.0.1.29 → 10.0.1.34 → 10.0.1.46 → 10.0.1.54 → 13.0.0.16
```
Path: EIGRP Zone → ASBR2 → OSPF Area 1 → ABR → OSPF Area 0 ✅

> Initial packet loss (25%) on first ping is expected — caused by ARP resolution on first transmission, not a routing issue.

---

## 🛠️ Technologies & Tools

- **Platform**: Cisco Packet Tracer
- **Devices**: Cisco 2911 Routers, Cisco 2960-24TT Switches
- **Protocols**: OSPF, EIGRP, RIPv2, 802.1Q, STP, DHCP
- **Concepts**: Route Redistribution, Router-on-a-Stick, VLAN Segmentation, Layer 2 Security, Centralized DHCP Relay

---

## 📁 Files

| File | Description |
|---|---|
| `Full-Routing-Protocol-Topology.pkt` | Main Packet Tracer project file |
| `Full-Routing-Protocol-Topology.png` | Network topology diagram |
| `README.md` | Project documentation |

---

## 👤 Author

**Mohamed Gamil Ibrahim El-Gafarawy**
Networking & Infrastructure Enthusiast | CCNA Track
📞 01008995417
> Built as a personal lab project to practice enterprise routing design and protocol interoperability.

---

## 📜 License

This project is open for educational use. Feel free to use it as a reference or learning resource.
