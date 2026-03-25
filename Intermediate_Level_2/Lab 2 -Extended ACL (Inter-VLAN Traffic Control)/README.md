# 📘 Lab 7 – Extended ACL (Inter-VLAN Traffic Control)

## 🎯 Objective

The goal of this lab is to implement **Extended Access Control Lists (ACLs)** on a Layer 3 switch to control traffic between VLANs and simulate real-world network security policies.

---

## 🧱 Topology

* 1 Layer 3 Switch (SVI-based routing)
* 3 VLANs:

  * VLAN 10 – HR
  * VLAN 20 – IT
  * VLAN 30 – Guest

---

## 🌐 IP Addressing Scheme

| VLAN | Name  | Network         | Gateway      |
| ---- | ----- | --------------- | ------------ |
| 10   | HR    | 192.168.10.0/24 | 192.168.10.1 |
| 20   | IT    | 192.168.20.0/24 | 192.168.20.1 |
| 30   | Guest | 192.168.30.0/24 | 192.168.30.1 |

---

## ⚙️ Configuration

### 🔹 Enable Layer 3 Routing

```
conf t
ip routing
```

---

### 🔹 Configure VLANs

```
vlan 10
name HR

vlan 20
name IT

vlan 30
name Guest
```

---

### 🔹 Assign Access Ports

```
interface fa0/1
switchport mode access
switchport access vlan 10

interface fa0/2
switchport mode access
switchport access vlan 20

interface fa0/3
switchport mode access
switchport access vlan 30
```

---

### 🔹 Configure SVI (Default Gateways)

```
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown

interface vlan 20
ip address 192.168.20.1 255.255.255.0
no shutdown

interface vlan 30
ip address 192.168.30.1 255.255.255.0
no shutdown
```

---

### 🔹 Configure Extended ACL

```
ip access-list extended GUEST-FILTER

! Block Guest access to internal VLANs
deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255

! Allow web access (HTTP/HTTPS)
permit tcp 192.168.30.0 0.0.0.255 any eq 80
permit tcp 192.168.30.0 0.0.0.255 any eq 443

! Deny all other traffic from Guest VLAN
deny ip 192.168.30.0 0.0.0.255 any

! Allow all other traffic
permit ip any any
```

---

### 🔹 Apply ACL to Guest VLAN

```
interface vlan 30
ip access-group GUEST-FILTER in
```

---

## 🧪 Verification

| Test Case          | Expected Result |
| ------------------ | --------------- |
| Guest → HR         | ❌ Blocked       |
| Guest → IT         | ❌ Blocked       |
| Guest → HTTP/HTTPS | ✅ Allowed       |
| HR/IT → Guest      | ✅ Allowed       |

---

## 📸 Suggested Screenshots

* Topology diagram
* `show vlan brief`
* `show ip interface brief`
* `show access-lists`
* Ping test results (success & failure)

---

## 🧠 Key Learnings

* Extended ACLs filter traffic based on:

  * Source IP
  * Destination IP
  * Protocol
  * Port number
* ACLs are processed **top-down**
* There is an **implicit deny** at the end of every ACL
* ACL placement is critical for performance and security
* Layer 3 switches can enforce routing and security policies together

---

## ❓ Interview Questions & Answers

### 1. Why apply ACL on VLAN 30 and not VLAN 10?

ACLs should be applied **closest to the source of unwanted traffic**. Since the Guest VLAN is the source of restricted traffic, applying the ACL on VLAN 30 ensures unwanted traffic is blocked early.

---

### 2. What happens if you remove `permit ip any any`?

All traffic not explicitly permitted will be **denied due to the implicit deny rule** at the end of the ACL. This can unintentionally block legitimate traffic from other VLANs.

---

### 3. Why does rule order matter?

ACLs are processed **top to bottom**, and the first matching rule is applied. Incorrect ordering can lead to unintended blocking or allowing of traffic.

---

### 4. Why wouldn’t a router ACL work in this topology?

The Layer 3 switch performs **inter-VLAN routing locally**, so traffic between VLANs never reaches the router. Therefore, applying ACLs on the router would not affect internal VLAN traffic.

---

## 🚀 Conclusion

This lab demonstrates how Extended ACLs can be used to implement **network segmentation and security policies**, similar to real-world firewall configurations.

---

