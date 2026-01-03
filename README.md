# Networking_Lab_Project7
Comprehensive network lab with advanced VLAN segmentation, DHCP relay, ACLs, and zone isolation, designed to simulate real-world enterprise network scenarios.

# Project 7 – Advanced VLAN Segmentation, ACLs and DHCP Relay

## Overview
This lab simulates a **real enterprise network** with segmented zones, centralized services,
and traffic control using ACLs.  
Focus is on **design decisions**, **security**, and **troubleshooting**, not just configuration.

---

## Network Objectives
- Implement multiple VLANs across access switches
- Separate the network into logical zones
- Centralize DHCP using relay (ip helper-address)
- Control inter-VLAN communication using ACLs
- Validate behavior through connectivity tests

---

## Network Zones

### Users Zone
- VLAN 10
- VLAN 20

### Servers Zone
- VLAN 30
- VLAN 40

### Management Zone
- VLAN 50
- VLAN 60

Zones are visually represented in Packet Tracer using **colored transparent areas**.

---

## VLAN and IP Addressing Plan

| VLAN | Zone        | Subnet              | Gateway            |
|------|-------------|---------------------|--------------------|
| 10   | Users       | 192.168.10.0/24     | 192.168.10.1       |
| 20   | Users       | 192.168.20.0/24     | 192.168.20.1       |
| 30   | Servers     | 192.168.30.0/24     | 192.168.30.1       |
| 40   | Servers     | 192.168.40.0/24     | 192.168.40.1       |
| 50   | Management  | 192.168.50.0/24     | 192.168.50.1       |
| 60   | Management  | 192.168.60.0/24     | 192.168.60.1       |

---

## Topology Summary

### Devices
- 2 Routers (R1, R2)
- 4 Access Switches
- 12 PCs (2 per VLAN)

### Design Decisions
- R1 hosts all DHCP pools (centralized DHCP)
- R2 handles VLAN 50 and 60 routing
- DHCP relay is used between R2 and R1
- Switches use access ports for PCs and trunk ports for routers

---

## DHCP Architecture

### Centralized DHCP
- All `ip dhcp pool` configurations are on **R1**
- VLANs 10–60 receive IP addresses from R1

### DHCP Relay
- VLANs connected to R2 use:
  - `ip helper-address <R1-IP>`
- R2 forwards DHCP requests to R1

This mirrors **real enterprise DHCP design**.

---

## Access Control Policy (ACL)

### Traffic Rules
- Users → Servers: ALLOWED
- Users → Management: DENIED
- Servers ↔ Servers: ALLOWED
- Management → Any: ALLOWED
- Any other traffic: DENIED

### Implementation
- Extended ACL 100
- Applied inbound on all VLAN subinterfaces
- Enforces strict zone separation

---

## Validation and Testing

### Tests Performed
- DHCP IP assignment on all VLANs
- Inter-VLAN ping tests (allowed traffic)
- Blocked ping tests (restricted traffic)
- DHCP binding verification on R1
- Router-to-router connectivity tests

### Result
All configurations behave as expected.

---

## Screenshot Documentation Order

All screenshots must follow this naming convention:

1. 01_topology_overview.png
2. 02_zone_visual_separation.png
3. 03_vlan_ip_plan.png
4. 04_switch1_vlan_config.png
5. 05_switch2_vlan_config.png
6. 06_switch3_vlan_config.png
7. 07_switch4_vlan_config.png
8. 08_switch_trunk_ports.png
9. 09_router1_subinterfaces.png
10. 10_router2_subinterfaces.png
11. 11_dhcp_pools_r1.png
12. 12_ip_helper_r2.png
13. 13_acl_configuration.png
14. 14_acl_applied_interfaces.png
15. 15_ping_users_to_servers.png
16. 16_ping_users_to_management_blocked.png
17. 17_dhcp_binding_verification.png

---

## Key Learnings
- VLANs require traffic control to be secure
- ACLs define real security boundaries
- DHCP relay is essential in segmented networks
- Documentation clarity is part of engineering quality

---

## Conclusion
This project represents an **advanced networking lab** that combines
segmentation, security, and centralized services.

It goes beyond basic VLAN configuration and reflects **real-world enterprise practices**.
