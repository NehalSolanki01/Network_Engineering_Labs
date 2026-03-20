# 🧪 Lab 7: Inter-VLAN Routing using Layer 3 Switch (SVI)

---

## 📌 Objective

Configure inter-VLAN routing using a **Layer 3 switch** by creating **Switch Virtual Interfaces (SVIs)** and enabling routing within the switch.

---

## 🧠 Key Concepts

* VLAN segmentation (Layer 2)
* Inter-VLAN routing (Layer 3)
* Switch Virtual Interface (SVI)
* `ip routing` on Layer 3 switches
* Default gateway using SVI

---

## 🏗️ Topology

```
PC1 (VLAN 10) ----\
                   \
                    L3 SWITCH (3560)
                   /
PC2 (VLAN 20) ----/
```

---

## 🧱 IP Addressing Plan

| Device | VLAN | IP Address   | Subnet Mask   | Default Gateway |
| ------ | ---- | ------------ | ------------- | --------------- |
| PC1    | 10   | 192.168.10.2 | 255.255.255.0 | 192.168.10.1    |
| PC2    | 20   | 192.168.20.2 | 255.255.255.0 | 192.168.20.1    |

---

## ⚙️ Configuration Steps

### 1️⃣ Create VLANs

```
enable
configure terminal

vlan 10
 name SALES

vlan 20
 name HR
```

---

### 2️⃣ Assign Ports to VLANs

```
interface fa0/1
 switchport mode access
 switchport access vlan 10

interface fa0/2
 switchport mode access
 switchport access vlan 20
```

---

### 3️⃣ Configure SVIs (Default Gateways)

```
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
```

---

### 4️⃣ Enable Layer 3 Routing

```
ip routing
```

---

## 🧪 Verification Commands

```
show vlan brief
show ip interface brief
show ip route
```

---

## ✅ Test Connectivity

From PC1:

```
ping 192.168.20.2
```

✔ Successful ping confirms inter-VLAN routing is working.

---

## 🧠 Packet Flow Explanation

1. PC1 sends traffic to its default gateway (192.168.10.1)
2. Switch receives traffic on VLAN 10
3. Switch routes internally using SVI
4. Packet is forwarded to VLAN 20
5. PC2 receives the packet

---

## 💡 Lessons Learned

* Layer 3 switches can perform routing without an external router
* SVIs act as default gateways for VLANs
* Physical switch ports operate at Layer 2 by default
* `ip routing` enables Layer 3 functionality on the switch
* Inter-VLAN routing on a Layer 3 switch is faster than Router-on-a-Stick

---
## 🏁 Conclusion

This lab demonstrates how to implement efficient inter-VLAN routing using a Layer 3 switch. It reflects real-world enterprise network design where routing is handled internally within switches for better performance and scalability.

---

