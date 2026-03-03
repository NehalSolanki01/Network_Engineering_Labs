# Lab 03 – Inter-VLAN Security with Extended ACL

## 🔎 Overview

This lab builds upon Lab 02 (Router-on-a-Stick) by introducing access control between VLANs using an Extended Access Control List (ACL).

The objective was to simulate a small enterprise network where a Guest VLAN is restricted from accessing internal departmental networks.

---

## 🎯 Objectives

- Extend network with VLAN 30 (Guest)
- Maintain inter-VLAN routing
- Implement extended ACL
- Enforce security policy
- Test inbound vs outbound ACL behavior
- Troubleshoot ACL misconfiguration scenarios

---

## 🏗 Network Topology

VLAN Structure:

| VLAN | Department | Subnet |
|------|------------|--------|
| 10 | SALES | 192.168.10.0/24 |
| 20 | IT | 192.168.20.0/24 |
| 30 | GUEST | 192.168.30.0/24 |

Router Subinterfaces:

- G0/0.10 → 192.168.10.1
- G0/0.20 → 192.168.20.1
- G0/0.30 → 192.168.30.1

---

## 🔐 Security Policy Implemented

| Source | Destination | Action |
|--------|------------|--------|
| Guest → Sales | Deny |
| Guest → IT | Deny |
| Guest → Gateway | Allow |
| Sales ↔ IT | Allow |

---

## 🔧 ACL Configuration

### Create Extended ACL

access-list 100 deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
access-list 100 deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
access-list 100 permit ip any any

**Apply ACL (Best Practice Placement)**
interface g0/0.30
ip access-group 100 in

Extended ACL placed inbound on Guest interface (close to source).

**🧪 Testing & Validation**

From Guest (192.168.30.10):

Ping 192.168.10.10 ❌

Ping 192.168.20.10 ❌

Ping 192.168.30.1 ✅

From Sales / IT:

VLAN 10 ↔ VLAN 20 communication remains functional ✅

**🛠 Troubleshooting Scenarios Explored**

Incorrect ACL rule order

Missing permit statement (implicit deny behavior)

ACL applied outbound instead of inbound

ACL applied on incorrect interface

ACL attached on multiple interfaces simultaneously

**Verified using:**
show access-list
show ip interface

**Key Concepts Demonstrated**

Extended ACL syntax (source + destination filtering)

Top-down ACL processing

Implicit deny rule

Inbound vs outbound filtering behavior

Correct ACL placement strategy

Packet flow analysis through router

Enterprise-style VLAN isolation

**📚 Lessons Learned**

ACL order determines behavior (first match wins).

Extended ACLs should be placed close to the source network.

Applying ACL in wrong direction can block return traffic.

Missing ACL entries result in implicit deny.

Always verify ACL attachment using show ip interface.

This lab simulates real-world VLAN security implementation in enterprise networks.
