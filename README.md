# Enterprise Network Topology Simulation

Welcome to my Cisco Packet Tracer Network lab! This project is a comprehensive simulation of an enterprise-grade network topology that I designed and configured using **Cisco Packet Tracer**. 

Building this topology was an incredible learning experience that allowed me to transition theoretical networking concepts into a fully functional, complex simulation. I implemented multi-area routing, high availability, redundancy, and strict security measures across different simulated corporate buildings.

If you are currently studying for your CCNA, exploring enterprise networking, or just looking for a comprehensive lab to study and replicate, feel free to dive into the details below!

---

## Network Architecture

* **West Building (OSPF Area 1):** Focuses on campus user access with ***multiple VLANs*** (VLAN 10, 20, 30, and 40) separating different departments.
* **Core Layer (OSPF Area 0):** The high-speed backbone connecting all buildings, ***managing redistribution***, and handling Network Address Translation (NAT) for internet-bound traffic.
* **Data Center Building (OSPF Area 2):** Houses critical enterprise servers (DHCP, DNS, RADIUS, HTTP, and NTP/Syslog) with dedicated high-availability paths.
* **South Building (EIGRP Area):** An independent building network utilizing ***EIGRP routing***, integrated with a Wireless LAN Controller (WLC) and Lightweight Access Points for Wi-Fi and IP Phone connectivity.

![Network Topology](./Network_Topology.png)

---

## Key Technical Highlights & Engineering Decisions

While building this, I ran into a few platform limitations within Packet Tracer, which forced me to get creative with my configurations. Here are the core implementations I am most proud of:

### 1. Smart Traffic Load Balancing (HSRP + STP Tuning)
> **The Challenge:** I wanted to use GLBP (Gateway Load Balancing Protocol) to dynamically balance traffic across my redundant distribution switches. However, Packet Tracer does not support GLBP.
>
> **The Workaround:** To achieve true load balancing without GLBP, I manually paired **HSRP v2** with **Spanning Tree Protocol (STP)** priority tuning. 
* I configured **DSW1** to be the Primary STP Root Bridge and Active HSRP Gateway for **VLAN 10 and 150**.
* I configured **DSW2** to be the Primary STP Root Bridge and Active HSRP Gateway for **VLAN 100 and 200**.
* This ensures that traffic from half the VLANs naturally flows through one switch, while the other half flows through the second, utilizing both hardware paths effectively while keeping a solid failover plan.

### 2. Optimizing Overhead with Passive Interfaces
To prevent unnecessary processing overhead, I heavily utilized the `passive-interface` command across the OSPF and EIGRP routing protocols. By blocking routing updates from being broadcasted down to end-user devices (like PCs, IP Phones, and Laptops), I ensured that:
* End devices aren't constantly processing useless routing packets.
* Network bandwidth is preserved.
* The network is more secure against rogue routers trying to form adjacencies on user ports.

### 3. Route Redistribution
Because the South Building relies on **EIGRP 100** and the rest of the network operates on **OSPF 1**, I configured mutual route redistribution at the Core Layer. I carefully injected OSPF routes into EIGRP (defining explicit metrics) and EIGRP subnets back into OSPF, establishing flawless end-to-end communication across the entire enterprise.

### 4. Link Aggregation & Security
* **LACP & PAgP:** Implemented EtherChannel across distribution and core switches to maximize bandwidth and provide link-level redundancy.
* **Switchport Security:** Enabled `STP Portfast`, `Root Guard`, and `BPDU Guard` alongside strict standard Port-Security mac-address limits on user-facing access ports to mitigate layer-2 attacks.

---

## Lessons Learned

* **Design for Limitations:** Working within Packet Tracer taught me that a good network engineer can achieve enterprise goals (like load balancing) even when their preferred protocol isn't supported by the hardware.
* **Documentation is Key:** Labeling subnets, IP assignments, and routing boundaries directly on the workspace layout saved me hours of troubleshooting when debugging routing loops and asymmetric routing paths.

---











## Technical Implementation & Features

| Technology Category | Feature | Implementation Details |
| :--- | :--- | :--- |
| **Dynamic Routing** | OSPF (Open Shortest Path First) | Configured globally for fast, automated route propagation. Optimized using `passive-interface` on user subnets and `default-information originate` to advertise the internet boundary. |
| **Switching Redundancy** | RPVST+ & EtherChannel | Layer 2 loop prevention with Rapid Per-VLAN Spanning Tree Plus. Bundled redundant trunk links into EtherChannels to maximize bandwidth utilization and failover speeds. |
| **Gateway Redundancy** | HSRP (Hot Standby Router Protocol) | Configured across critical VLANs to provide instant default gateway failover and uninterrupted user access. |
| **Network Security** | Layer 2 & Management Hardening | Enforced `Port-Security` at the access layer. Secured edge switches with `Portfast` and `BPDUGuard` to block rogue switches. Enabled secure management via encrypted SSH access. <br><br> Adhered to ***CCNA security best practices by deprecating VLAN 1*** for user traffic, isolating all unused/leftover ports into a dedicated VLAN Parking Lot, and executing a blanket administrative shutdown (shutdown) on those ports to eliminate unauthorized physical access. |
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


