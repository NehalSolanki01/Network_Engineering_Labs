# Lab 4 – DHCP per VLAN (Router-on-a-Stick)

**Skills Demonstrated**

- VLAN configuration
- Router-on-a-Stick
- 802.1Q trunking
- DHCP configuration
- Inter-VLAN routing
- Network troubleshooting

## Objective

The goal of this lab is to configure Dynamic Host Configuration Protocol (DHCP) for multiple VLANs in a segmented network. The router acts both as the default gateway for each VLAN and as the DHCP server that automatically assigns IP addresses to hosts.

This lab demonstrates how VLAN segmentation, trunking, and inter-VLAN routing work together with DHCP to dynamically allocate IP addresses to devices in different networks.

---

## Topology

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/68bf9174-cac0-4235-98bd-8d27c3ad673d" />


* 1 Router
* 1 Layer 2 Switch
* 4 PCs

Connections:

PC0 → Switch Fa0/1
PC1 → Switch Fa0/2
PC2 → Switch Fa0/3
PC3 → Switch Fa0/4

Switch Fa0/24 → Router G0/0 (Trunk Link)

---

## VLAN and Network Design

| VLAN | Name  | Network         | Gateway      |
| ---- | ----- | --------------- | ------------ |
| 10   | SALES | 192.168.10.0/24 | 192.168.10.1 |
| 20   | HR    | 192.168.20.0/24 | 192.168.20.1 |

PC0 and PC2 belong to VLAN 10.
PC1 and PC3 belong to VLAN 20.

Each VLAN represents a separate Layer 2 broadcast domain and therefore requires a separate IP subnet and DHCP pool.

---

## Step 1 – Configure VLANs on the Switch

```bash
enable
configure terminal

vlan 10
name SALES

vlan 20
name HR
```

Assign access ports:

```bash
interface range fa0/1 , fa0/3
switchport mode access
switchport access vlan 10

interface range fa0/2 , fa0/4
switchport mode access
switchport access vlan 20
```

Configure trunk port toward router:

```bash
interface fa0/24
switchport mode trunk
```

---

## Step 2 – Configure Router-on-a-Stick

Enable router interface:

```bash
interface g0/0
no shutdown
```

Create subinterface for VLAN 10:

```bash
interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

Create subinterface for VLAN 20:

```bash
interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

Each subinterface acts as the default gateway for its VLAN.

---

## Step 3 – Configure DHCP Pools on Router

Exclude gateway addresses:

```bash
ip dhcp excluded-address 192.168.10.1
ip dhcp excluded-address 192.168.20.1
```

Create DHCP pool for VLAN 10:

```bash
ip dhcp pool VLAN10
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
```

Create DHCP pool for VLAN 20:

```bash
ip dhcp pool VLAN20
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 8.8.8.8
```

The router will now automatically assign IP addresses to devices within each VLAN.

---

## Verification

Router commands:

```bash
show ip dhcp binding
show ip dhcp pool
show ip interface brief
```

Switch commands:

```bash
show vlan brief
show interfaces trunk
```

PC verification:

* VLAN 10 hosts receive IP addresses from the 192.168.10.0/24 network.
* VLAN 20 hosts receive IP addresses from the 192.168.20.0/24 network.

Inter-VLAN communication is successful through the router.

---

## Key Concepts Learned

* VLAN segmentation
* Router-on-a-Stick architecture
* 802.1Q trunking
* DHCP scope per subnet
* Inter-VLAN routing

Each VLAN functions as a separate broadcast domain and requires its own subnet and DHCP pool.

---

## Future Improvements

In enterprise networks, DHCP services are typically provided by centralized servers (such as Windows Server or Infoblox). Routers or Layer 3 switches forward DHCP requests using the `ip helper-address` command.

This concept will be implemented in the next lab.

