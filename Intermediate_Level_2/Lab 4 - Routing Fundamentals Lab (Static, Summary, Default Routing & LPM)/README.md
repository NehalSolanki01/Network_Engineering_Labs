# 🔹 Routing Fundamentals Lab (Static, Summary, Default Routing & LPM)

## 📌 Overview

This lab demonstrates core IP routing concepts including:

* Connected & Local Routes
* Static Routing
* Route Summarization
* Default Routing
* Longest Prefix Match (LPM)
* Load Balancing (Equal-Cost Paths)

The lab was implemented using Cisco Packet Tracer.

---

## 🧠 Objectives

* Understand how routers learn connected and local routes
* Configure static routes for end-to-end connectivity
* Implement route summarization to simplify routing tables
* Configure default routes for unknown destinations
* Analyze traffic paths using traceroute
* Understand longest prefix match behavior
* Configure basic load balancing using multiple default routes

---

## 🏗️ Topology

* 5 Routers (R1–R5)
* 2 Switches
* 3 PCs
* Multiple subnets (10.0.x.x and 10.1.x.x)

---

## ⚙️ Key Configurations

### 1. Interface Configuration

* Assigned IP addresses to all router interfaces
* Enabled interfaces using `no shutdown`

---

### 2. Connected & Local Routes

* Verified using:

```
show ip route
```

* Observed:

  * `C` → Connected routes
  * `L` → Local routes

---

### 3. Static Routing

* Configured routes between all networks:

```
ip route <network> <mask> <next-hop>
```

* Verified full connectivity using ping between:

  * PC1 ↔ PC2
  * PC1 ↔ PC3

---

### 4. Route Summarization

* Removed individual static routes on R1
* Added summary route:

```
ip route 10.1.0.0 255.255.0.0 10.0.0.2
```

* Reduced routing table size while maintaining connectivity

---

### 5. Longest Prefix Match (LPM)

* Added specific route:

```
ip route 10.1.3.0 255.255.255.0 10.0.3.2
```

* Router preferred /24 over /16 route

---

### 6. Default Routing

* Configured default routes:

```
ip route 0.0.0.0 0.0.0.0 <next-hop>
```

* Provided internet-like connectivity across the topology

---

### 7. Load Balancing

* Configured multiple default routes on R1:

```
ip route 0.0.0.0 0.0.0.0 10.0.0.2
ip route 0.0.0.0 0.0.0.0 10.0.3.2
```

* Traffic was load balanced across equal-cost paths

---

## 📊 Key Learnings

* Routes only appear when interfaces are up
* Static routes provide full control but are not scalable
* Summarization reduces routing table size
* Longest prefix match determines route selection
* Return path may differ from forward path (asymmetric routing)
* Load balancing occurs when equal-cost routes exist


## Q1: What is the difference between connected and local routes?

**Answer:**
Connected routes represent directly connected networks, while local routes represent the router’s own interface IP addresses.

---

### Q2: What is a static route?

**Answer:**
A manually configured route that tells a router how to reach a specific network.

---

### Q3: What is route summarization?

**Answer:**
Combining multiple routes into a single route to reduce routing table size.

---

### Q4: What is longest prefix match?

**Answer:**
Routers choose the route with the most specific match (largest subnet mask).

---

### Q5: What is a default route?

**Answer:**
A fallback route used when no specific route exists (0.0.0.0/0).

---

### Q6: Why can traffic take different return paths?

**Answer:**
Because routing decisions are made independently on each router.

---

## 📚 Learning Outcome

This lab strengthened my understanding of routing logic, traffic flow, and how routers make forwarding decisions in real-world networks.

---

