# Networking_Lab_Project7
Comprehensive network lab with advanced VLAN segmentation, DHCP relay, ACLs, and zone isolation, designed to simulate real-world enterprise network scenarios.

Lab Overview
Routers: 2 (R1 and R2)
Switches: 4
VLANs: 6
Users: VLAN 10, VLAN 20
Servers: VLAN 30, VLAN 40
Management: VLAN 50, VLAN 60
PCs per VLAN: 2
Key Concepts:
DHCP centralized with relay (IP helper)
Inter-VLAN routing
ACLs controlling who can communicate with whom
VLAN isolation for sensitive networks
Gateway redundancy (conceptual HSRP)
Simulated link failure
Goals: Practice enterprise-level configuration, inter-VLAN security, and network troubleshooting.
Topology Overview
4 access switches connected to 2 routers.
VLANs assigned as follows:
VLAN 10 & 20 → User PCs
VLAN 30 & 40 → Servers
VLAN 50 & 60 → Management/isolated PCs
IP addressing scheme per VLAN is set on the routers using DHCP pools.
Switch ports configured as access ports for PCs and trunk ports for inter-switch links.
Step-by-Step Configuration
VLANs Configuration on Switches
Copiar código
Text
vlan 10
name Users_VLAN10
vlan 20
name Users_VLAN20
vlan 30
name Servers_VLAN30
vlan 40
name Servers_VLAN40
vlan 50
name Management_VLAN50
vlan 60
name Management_VLAN60
Access Ports Assignment
Copiar código
Text
interface fa0/1
 switchport mode access
 switchport access vlan 10
interface fa0/2
 switchport mode access
 switchport access vlan 20
... (repeat for all PCs)
Trunk Ports Configuration
Copiar código
Text
interface fa0/24
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40,50,60
Router Subinterfaces & IP Addresses
Copiar código
Text
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
...
interface g0/1.50
 encapsulation dot1Q 50
 ip address 192.168.50.1 255.255.255.0
interface g0/1.60
 encapsulation dot1Q 60
 ip address 192.168.60.1 255.255.255.0
DHCP Pools on Router 1
Copiar código
Text
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
!
ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8
...
DHCP Relay (IP Helper) on Router 2
Copiar código
Text
interface g0/0
 ip helper-address 192.168.10.1
 ip helper-address 192.168.20.1
 ip helper-address 192.168.30.1
 ip helper-address 192.168.40.1
ACL Configuration
Copiar código
Text
access-list 100 permit ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255
access-list 100 permit ip 192.168.20.0 0.0.0.255 192.168.40.0 0.0.0.255
access-list 100 permit ip 192.168.50.0 0.0.0.255 any
access-list 100 permit ip 192.168.60.0 0.0.0.255 any
access-list 100 deny ip any any
!
interface g0/0.10
 ip access-group 100 in
... (repeat for all subinterfaces)
Testing & Verification
Ping between PCs in the same VLAN and across allowed VLANs
Ping should fail between isolated VLANs (e.g., VLAN 50 → VLAN 10 blocked)
Verify DHCP leases:
Copiar código
Text
show ip dhcp binding
