## 3-Tier Network Lab (Cisco Packet Tracer)

A complete enterprise-grade networking simulation project built in **Cisco Packet Tracer**, implementing a 3-Tier Campus Architecture across two buildings — a Campus and a Data Center. This lab mirrors real-world networking environments with emphasis on scalability, security, redundancy, and high availability.

---

## Overview

This lab project models a structured enterprise network with multiple departments, services, and secure routing/switching practices. It's built using a **core-distribution-access** layered approach for clear segmentation and efficient management.

### Architecture

- **3-Tier Campus Model**
- **1 Data Center**
- **1 Campus Building**
- **Redundant Links + High Availability**

![Logical Topology](./Topology.png)

---

## Technical Features

| Feature | Description |
|--------|--------------------------------|
| ✅ **Dynamic Routing** | OSPF configured across the network for route sharing. Default information originate, passive interface |
| ✅ **Switching Redundancy** | RPVST+ and EtherChannel between core/distribution layers |
| ✅ **Security** | Port-Security, Portfast and BPDUGuard, Secure SSH access |
| ✅ **High Availability** | HSRP for gateway redundancy across key VLANs |
| ✅ **Network Services** | Internal DHCP, DNS, and HTTP servers deployed |
| ✅ **VLANs** | Structured VLANs (10–99) including Management VLAN |
| ✅ **NAT** | Static NAT for servers and PAT for user Internet access |

---
