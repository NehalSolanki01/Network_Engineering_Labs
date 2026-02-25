**Lab Overview**

This lab demonstrates the implementation of inter-VLAN routing using a single physical router interface (Router-on-a-Stick architecture).

The objective was to simulate a small enterprise network where different departments are segmented using VLANs but still require controlled Layer 3 communication.

This lab focuses on:

VLAN segmentation

802.1Q trunking

Subinterface configuration

Inter-VLAN routing

Layer 2 vs Layer 3 behavior

ARP and frame encapsulation analysis

Structured troubleshooting

**Network Topology**

Devices Used

Cisco 2960 Switch

Cisco 2911 Router

2 End Devices (PCs)

**Logical Segmentation**

VLAN	Department	Subnet
10	SALES	192.168.10.0/24
20	IT	192.168.20.0/24

Single trunk link:
Switch Gi0/1  <------>  Router Gi0/0

**Design Rationale**

Instead of using two physical router interfaces (as in Lab 01), this lab uses:

One physical interface

Multiple logical subinterfaces

VLAN tagging (802.1Q)

This design reflects how small and medium enterprise networks are commonly implemented to reduce hardware usage while maintaining logical segmentation.

Best practice followed:

One VLAN = One Subnet

**IP Addressing Plan**
Device	Interface	VLAN	IP Address
PC0	Fa0/1	10	192.168.10.10
PC1	Fa0/2	20	192.168.20.10
Router	G0/0.10	10	192.168.10.1
Router	G0/0.20	20	192.168.20.1

Default gateways configured per VLAN.

**Switch Configuration**
**Create VLANs**
enable
configure terminal
vlan 10
 name SALES
vlan 20
 name IT
 
**Assign Access Ports**
interface fa0/1
 switchport mode access
 switchport access vlan 10

interface fa0/2
 switchport mode access
 switchport access vlan 20
 
**Configure Trunk**
interface gigabitEthernet0/1
 switchport mode trunk

**Verification:**

show interfaces trunk
show vlan brief

**Router Configuration (Router-on-a-Stick)**

**Enable physical interface:**

interface gigabitEthernet0/0
 no shutdown
Subinterface for VLAN 10
interface gigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
Subinterface for VLAN 20
interface gigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

**Verification:**

show ip interface brief
show ip route

**Testing**

From PC0:

ping 192.168.10.1   ✅
ping 192.168.20.10  ✅

From PC1:

ping 192.168.20.1   ✅
ping 192.168.10.10  ✅

Inter-VLAN routing successfully established.

