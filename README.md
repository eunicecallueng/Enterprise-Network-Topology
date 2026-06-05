# 3-Tier Enterprise Campus & Data Center Network Lab

Welcome to my Cisco Packet Tracer networking project! This lab was built to bridge the gap between CCNA theory and practical application. It simulates a realistic corporate environment across two main sectors—a **Campus Building** and a **Data Center**—utilizing a structured 3-tier architecture. 

If you are currently studying for your CCNA, exploring enterprise networking, or just looking for a comprehensive lab to study and replicate, feel free to dive into the details below!

---

## Network Architecture

The project is structured around the classic **Core-Distribution-Access** layered model, engineered to handle real-world challenges like scalability, security, and zero-downtime high availability.

* **Campus Building:** Houses multiple department networks segmented into structured VLANs.
* **Data Center (DC):** Hosts localized corporate network services (HTTP, DNS, DHCP, NTP) protected by dedicated infrastructure.
* **Redundant Infrastructure:** Multi-homed connections linking the Core and Distribution tiers to prevent single points of failure.

![Logical Topology](./Topology.png)

---

## Technical Implementation & Features

| Technology Category | Feature | Implementation Details |
| :--- | :--- | :--- |
| **Dynamic Routing** | OSPF (Open Shortest Path First) | Configured globally for fast, automated route propagation. Optimized using `passive-interface` on user subnets and `default-information originate` to advertise the internet boundary. |
| **Switching Redundancy** | RPVST+ & EtherChannel | Layer 2 loop prevention with Rapid Per-VLAN Spanning Tree Plus. Bundled redundant trunk links into EtherChannels to maximize bandwidth utilization and failover speeds. |
| **Gateway Redundancy** | HSRP (Hot Standby Router Protocol) | Configured across critical VLANs to provide instant default gateway failover and uninterrupted user access. |
| **Network Security** | Layer 2 & Management Hardening | Enforced `Port-Security` at the access layer. Secured edge switches with `Portfast` and `BPDUGuard` to block rogue switches. Enabled secure management via encrypted SSH access. <br><br> Adhered to **CCNA security best practices by deprecating VLAN 1** for user traffic, isolating all unused/leftover ports into a dedicated VLAN Parking Lot, and executing a blanket administrative shutdown (shutdown) on those ports to eliminate unauthorized physical access. |
| **Boundary Protocols** | NAT & PAT | Implemented Static NAT to safely map internal servers to public IPs, and Port Address Translation (PAT) for internal users accessing the Internet. |
| **Core Services** | Data Center Infrastructure | Centralized provisioning via internal DHCP (IP assignment), DNS (name resolution), and HTTP servers. |

---

## VLAN Segmentation Guide

The network uses a highly structured VLAN schema (ranging from VLAN 10 to 99) designed to cleanly segment broadcast domains and apply granular security policies:
* **Departmental Traffic:** Divided into distinct subnets for users, management, and guests.
* **Management VLAN:** A dedicated, isolated VLAN explicitly used for secure remote switch and router configuration.

---

## How to Explore or Replicate This Lab

1. **Clone or Download:** Grab the `.pkt` file from this repository.
2. **Launch:** Open it using **Cisco Packet Tracer** (v8.0 or higher recommended).
3. **Verify Configurations:** * Run `show ip route` to check the OSPF routing table.
   * Run `show standby brief` to inspect active/standby HSRP states.
   * Run `show etherchannel summary` to verify bundled trunk lines.
4. **Test Connectivity:** Try pinging from a Campus Client PC to the Data Center HTTP Server or simulating a link failure to watch the network automatically self-heal via RPVST+ and HSRP

---


I built this lab as part of my deep-dive journey into networking engineering. If you are preparing for your CCNA or working on your own portfolio projects, feel free to fork this repository

## *Happy Labbing!*


