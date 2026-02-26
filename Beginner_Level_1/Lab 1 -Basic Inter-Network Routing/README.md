
# Lab 1 – Basic Inter-Network Routing

## Project Overview

This lab demonstrates basic Layer 3 routing using a Cisco router in Packet Tracer. 

The objective was to enable communication between two separate IPv4 networks by correctly configuring router interfaces and end devices.

This lab focuses on foundational CCNA concepts including:

- IPv4 addressing
- Subnetting (/24 networks)
- Default gateway configuration
- Router interface configuration
- Troubleshooting connectivity issues
- Using Cisco CLI operational modes

---

## Scenario

A small office has two departments placed in separate IP networks:

- Network A: 192.168.1.0/24
- Network B: 192.168.2.0/24

Requirement:
Devices in both networks must be able to communicate with each other through a router.

---

## Topology

PC0 → Switch → Router → Switch → PC1

Two physical router interfaces were used:
- GigabitEthernet0/0 → Network 192.168.1.0
- GigabitEthernet0/1 → Network 192.168.2.0

---

---

## Router Configuration
enable
configure terminal

interface gigabitEthernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
exit


---

## Verification Steps

- `show ip interface brief`
- Ping test from PC0 to 192.168.1.1
- Ping test from PC0 to 192.168.2.10

Successful bidirectional communication confirmed proper routing.

---

## Troubleshooting Experience

During the lab, the following issues were encountered:

- Router interfaces were administratively down by default.
- Incorrect IP address was initially assigned to the wrong interface.
- Ping to default gateway failed due to misconfiguration.
- CLI privilege levels prevented configuration commands.

Issues were resolved using:
- `no shutdown`
- Correct interface IP assignment
- `show ip interface brief` for verification
- Proper navigation between EXEC and configuration modes

---

## Key Learning Outcomes

- Router interfaces are disabled by default.
- Each router interface represents a different broadcast domain.
- End devices must have correct default gateway to reach remote networks.
- Verification commands are essential for troubleshooting.
- Physical layer (cabling) directly impacts logical connectivity.

---

## Skills Demonstrated

- Basic network design
- IPv4 configuration
- Cisco CLI usage
- Layer 3 troubleshooting
- Logical problem isolation
- Documentation of technical work

  ## 📚 Lessons Learned

- A router interface must be manually enabled (`no shutdown`) before it can forward traffic.
- The default gateway on a PC must match the exact IP address of the router interface in that subnet.
- Subnet masks determine whether traffic is considered local or remote.
- ARP is triggered only after the Layer 3 decision is made (local → ARP for host, remote → ARP for gateway).
- Directly connected routes are automatically added to the routing table once an interface is configured and up.
- IP addresses remain end-to-end, while MAC addresses are rewritten at every router hop.
- Effective troubleshooting follows a layered approach: Physical → Interface State → IP Configuration → Routing Table.
- First ping failure is often due to ARP resolution and is expected behavior.

This lab strengthened my understanding of fundamental routing logic, subnet boundaries, and the interaction between Layer 2 and Layer 3.

--------------
