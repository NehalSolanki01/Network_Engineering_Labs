# 📘 Lab 3 – OSPF (Single Area, Multi-Router)

## 🎯 Objective

The objective of this lab is to configure **OSPF (Open Shortest Path First)** as a dynamic routing protocol and enable routers to automatically learn and exchange routing information.

---

## 🧱 Topology

* 3 Routers (R1, R2, R3)
* 2 End Devices (PC1, PC2)
* Linear topology:

```
PC1 --- R1 --- R2 --- R3 --- PC2
```

---

## 🌐 IP Addressing Scheme

### 🔹 LAN Networks

| Device | Interface | IP Address   | Subnet        |
| ------ | --------- | ------------ | ------------- |
| R1     | G0/1      | 192.168.1.1  | 255.255.255.0 |
| PC1    | Fa0       | 192.168.1.10 | 255.255.255.0 |
| R3     | G0/1      | 192.168.3.1  | 255.255.255.0 |
| PC2    | Fa0       | 192.168.3.10 | 255.255.255.0 |

---

### 🔹 Router-to-Router Links

| Link  | Interface | IP Address | Subnet          |
| ----- | --------- | ---------- | --------------- |
| R1–R2 | R1 G0/2   | 10.0.12.1  | 255.255.255.252 |
| R1–R2 | R2 G0/0   | 10.0.12.2  | 255.255.255.252 |
| R2–R3 | R2 G0/1   | 10.0.23.1  | 255.255.255.252 |
| R2–R3 | R3 G0/2   | 10.0.23.2  | 255.255.255.252 |

---

## ⚙️ Configuration

### 🔹 Configure Interfaces

#### R1

```
conf t
interface g0/1
ip address 192.168.1.1 255.255.255.0
no shutdown

interface g0/2
ip address 10.0.12.1 255.255.255.252
no shutdown
```

---

#### R2

```
conf t
interface g0/0
ip address 10.0.12.2 255.255.255.252
no shutdown

interface g0/1
ip address 10.0.23.1 255.255.255.252
no shutdown
```

---

#### R3

```
conf t
interface g0/1
ip address 192.168.3.1 255.255.255.0
no shutdown

interface g0/2
ip address 10.0.23.2 255.255.255.252
no shutdown
```

---

### 🔹 Configure OSPF

#### R1

```
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
network 10.0.12.0 0.0.0.3 area 0
```

---

#### R2

```
router ospf 1
network 10.0.12.0 0.0.0.3 area 0
network 10.0.23.0 0.0.0.3 area 0
```

---

#### R3

```
router ospf 1
network 10.0.23.0 0.0.0.3 area 0
network 192.168.3.0 0.0.0.255 area 0
```

---

## 🧪 Verification

### 🔹 Check OSPF Neighbors

```
show ip ospf neighbor
```

Expected:

* R1 ↔ R2
* R2 ↔ R1 and R3
* R3 ↔ R2

---

### 🔹 Check Routing Table

```
show ip route
```

Look for:

```
O 192.168.3.0/24
```

---

### 🔹 Test Connectivity

* PC1 → PC2 → ✅ Successful
* R1 → 192.168.3.1 → ✅ Successful

---

## 🧠 Key Learnings

* OSPF is a dynamic routing protocol
* Routers form neighbor relationships using Hello packets
* Routes are learned automatically without static configuration
* OSPF uses Area 0 as the backbone
* “O” in routing table indicates OSPF-learned routes
* OSPF requires IP connectivity before forming neighbors

---

### 1. Why did OSPF not work before fixing connectivity?

OSPF depends on IP connectivity. If routers cannot ping each other, neighbor relationships cannot form.

---

### 2. What does “O” in routing table represent?

It indicates that the route was learned via OSPF.

---

### 3. Why must Area IDs match?

Routers must be in the same OSPF area to form neighbor relationships and exchange routing information.

---

### 4. Why is OSPF better than static routing?

OSPF automatically updates routes when the network changes, making it scalable and efficient.

---

## 🚀 Conclusion

This lab demonstrates how OSPF enables routers to dynamically exchange routing information, improving scalability and reducing manual configuration effort in networks.

---

