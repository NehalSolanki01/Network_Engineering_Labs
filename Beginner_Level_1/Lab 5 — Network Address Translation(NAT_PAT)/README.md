**Lab 5 — Network Address Translation (NAT / PAT)**
**Overview**

This lab demonstrates the configuration of Network Address Translation (NAT) using Port Address Translation (PAT) on a Cisco router. NAT allows multiple internal devices using private IP addresses to access external networks through a single public IP address.

The lab extends the previous network topology that includes VLAN segmentation and router-on-a-stick routing.

**Objectives**

Configure NAT Overload (PAT) on a Cisco router

Translate private IP addresses to a public IP

Enable communication between internal VLAN networks and an external network

Verify NAT translations using router commands

Understand how packets are translated when leaving the internal network

**IP Addressing Scheme**

Device	Interface	IP Address	Description

R1	G0/0.10	192.168.10.1	VLAN 10 Gateway

R1	G0/0.20	192.168.20.1	VLAN 20 Gateway

R1	G0/0.30	192.168.30.1	VLAN 30 Gateway

R1	G0/1	203.0.113.1	Public NAT address

ISP	G0/0	203.0.113.254	Simulated Internet

PC1	DHCP	192.168.10.x	VLAN 10 host

PC2	DHCP	192.168.20.x	VLAN 20 host

PC3	DHCP	192.168.30.x	VLAN 30 host

**Configuration
Step 1 — Configure Router Subinterfaces**


interface g0/0

no shutdown

interface g0/0.10

encapsulation dot1Q 10

ip address 192.168.10.1 255.255.255.0

ip nat inside

interface g0/0.20

encapsulation dot1Q 20

ip address 192.168.20.1 255.255.255.0

ip nat inside

interface g0/0.30

encapsulation dot1Q 30

ip address 192.168.30.1 255.255.255.0

ip nat inside

**Step 2 — Configure Outside Interface**

interface g0/1

ip address 203.0.113.1 255.255.255.0

ip nat outside

no shutdown


**Step 3 — Configure NAT Access List**

access-list 1 permit 192.168.0.0 0.0.255.255

This ACL defines which internal networks are allowed to be translated.

**Step 4 — Enable NAT Overload (PAT)**

ip nat inside source list 1 interface g0/1 overload

This command allows multiple internal hosts to share one public IP address.

**Step 5 — Configure Default Route**

ip route 0.0.0.0 0.0.0.0 203.0.113.254

This route directs traffic toward the ISP router.

**Verification**

**Test Connectivity**

From a PC:

ping 203.0.113.254

Successful replies confirm connectivity between internal networks and the external router.

**Verify NAT Translations**

Run on the router:

show ip nat translations

**Example output:**

Pro Inside global      Inside local      Outside local      Outside global

icmp 203.0.113.1:45    192.168.10.2:45   203.0.113.254      203.0.113.254

This confirms that the private address 192.168.10.2 is translated to 203.0.113.1.

**NAT Statistics**

show ip nat statistics

This command displays NAT usage and active translation counts.

**Packet Flow Explanation**

A PC sends traffic to an external destination.

192.168.10.2 → 203.0.113.254

The router translates the private address.

192.168.10.2 → 203.0.113.1

The ISP router receives the packet and replies.

The router translates the packet back to the internal host.

**Skills Demonstrated**

VLAN segmentation
Inter-VLAN routing
Router-on-a-stick configuration
NAT (PAT) implementation
ACL configuration for NAT
Default routing
Network troubleshooting

**Lessons Learned**

During this lab several important networking concepts and troubleshooting techniques were reinforced.

**1. NAT Requires Traffic to Create Translations**

Initially, the NAT translation table showed zero entries. This highlighted that NAT translations are only created when traffic flows from an inside network to an outside network.

Command used to verify:
show ip nat translations

**2. Correct Interface Roles Are Critical**

NAT requires interfaces to be clearly defined as:
Inside interfaces → internal VLAN networks
Outside interface → connection to the external network (ISP)

Example configuration:
ip nat inside
ip nat outside
Incorrect interface assignment prevents NAT from functioning.

**3. Devices Must Be in the Same Subnet to Communicate**
A misconfigured IP address on the ISP router (203.0.11.254 instead of 203.0.113.254) caused connectivity failure. This demonstrated that devices must belong to the same subnet to communicate directly.

Correct addressing:
R1 g0/1  → 203.0.113.1
ISP g0/0 → 203.0.113.254
Subnet   → 255.255.255.0

**4. Routers Cannot Have Two Interfaces in the Same Network**
Attempting to configure multiple interfaces with addresses in the same subnet produced an overlapping network error.

Example error:
% 203.0.113.0 overlaps with GigabitEthernet0/1
Routers require each interface to belong to a different network unless bridging is configured.

**5. Step-by-Step Connectivity Testing Helps Identify Problems**
Troubleshooting was performed progressively:

PC → Gateway
PC → Router outside interface
Router → ISP router

Example tests:
ping 192.168.10.1
ping 203.0.113.1
ping 203.0.113.254
This approach helps isolate where connectivity fails.

**6. NAT Translation Table Helps Validate Configuration**

The NAT translation table confirmed that private IP addresses were successfully translated into a public IP address using PAT.

Example output:
Inside local: 192.168.10.2
Inside global: 203.0.113.1

This demonstrates how multiple internal devices can share a single public IP address.

**Key Takeaway**

This lab reinforced how NAT, routing, VLANs, and troubleshooting are interconnected in real networks. Correct addressing, interface roles, and step-by-step verification are essential for successful network deployment and debugging.
