# Cisco Switches & Routers Device Functions

## Overview

This lab explores fundamental networking concepts related to **Layer 2 switching and Layer 3 routing** using Cisco devices.

The focus of this exercise is to understand:

* How switches learn MAC addresses
* How routers build routing tables
* How packets travel across a network
* How static routes are configured

This lab was implemented using **Cisco Packet Tracer**.

---

## Lab Topology

Devices used in the lab:

* 4 Routers (R1, R2, R3, R4)
* 2 Switches (SW1, SW2)

All routers are connected through switches in the **10.10.10.0/24 network**.

Example IP addressing:

| Device | Interface | IP Address |
| ------ | --------- | ---------- |
| R1     | G0/0      | 10.10.10.1 |
| R2     | G0/0      | 10.10.10.2 |
| R3     | G0/1      | 10.10.10.3 |
| R4     | G0/0      | 10.10.10.4 |

---

## Lab Objectives

This lab demonstrates:

1. Viewing router interfaces and IP addresses
2. Identifying MAC addresses on interfaces
3. Verifying connectivity using ping
4. Viewing MAC address tables on switches
5. Clearing dynamic MAC entries
6. Examining router routing tables
7. Activating router interfaces
8. Configuring static routes

---

## Commands Used

### Verify Router Interfaces

```
show ip interface brief
```

Displays interface status and configured IP addresses.

---

### Check MAC Address of Interfaces

```
show interface gig0/0
```

Shows the hardware MAC address of an interface.

---

### Test Connectivity

```
ping <destination-ip>
```

Used to verify end-to-end network connectivity.

---

### View Switch MAC Address Table

```
show mac address-table dynamic
```

Displays dynamically learned MAC addresses and the ports they are associated with.

---

### Clear MAC Address Table

```
clear mac address-table dynamic
```

Removes dynamically learned MAC addresses.

---

### View Routing Table

```
show ip route
```

Displays the router's routing table including connected and static routes.

---

### Enable Router Interface

```
no shutdown
```

Brings an administratively down interface online.

---

### Configure Static Route

```
ip route 10.10.30.0 255.255.255.0 10.10.10.2
```

Adds a static route to reach the 10.10.30.0/24 network via next-hop router 10.10.10.2.

---

## Key Concepts Learned

### Switch MAC Learning

Switches dynamically learn the source MAC address of incoming frames and store them in the MAC address table.

This allows switches to forward frames only to the correct port rather than flooding traffic.

---

### Routing Table Entries

Routers maintain routing tables containing:

* **Connected routes (C)** – networks directly attached to the router
* **Local routes (L)** – the router's own interface IP
* **Static routes (S)** – manually configured routes

---

### Interface State

Router interfaces are **administratively shutdown by default** and must be manually enabled using:

```
no shutdown
```

---

## Lessons Learned

* Switches build MAC address tables dynamically by observing traffic.
* Routers automatically add connected networks to their routing tables.
* Interfaces must be active for routes to appear in the routing table.
* Static routes allow routers to reach remote networks.

---

## Tools Used

* Cisco Packet Tracer
* Cisco IOS CLI

