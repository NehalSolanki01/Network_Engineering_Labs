# 🔧 Cisco Device Management Lab

## 📌 Overview

This lab demonstrates essential Cisco device management tasks including factory reset, password recovery, configuration backup, IOS image backup/recovery, and IOS upgrade.

These are real-world skills required for network engineers and cybersecurity professionals.

---

## 🧠 Objectives

* Perform factory reset on a Cisco router
* Recover lost passwords using ROMMON
* Backup and restore configurations (Flash & TFTP)
* Backup and recover IOS images
* Upgrade IOS on a Cisco switch

---

## 🛠️ Lab Setup

* **Router:** R1
* **Switch:** SW1
* **TFTP Server IP:** 10.10.10.10
* **Network:** 10.10.10.0/24

---

## ⚙️ Configuration Steps

### 1️⃣ Factory Reset

```
write erase
reload
```

✔️ Cleared all configurations and rebooted the router

---

### 2️⃣ Restore Base Configuration

```
hostname R1
interface g0/0
ip address 10.10.10.1 255.255.255.0
no shutdown
```

---

### 3️⃣ Password Recovery

```
config-register 0x2142
reload
```

In ROMMON:

```
confreg 0x2142
reset
```

After boot:

```
copy start run
```

✔️ Startup config preserved
✔️ Running config initially empty

---

### 4️⃣ Fix Interface Issue

```
interface g0/0
no shutdown
```

✔️ Interfaces are shutdown by default

---

### 5️⃣ Configuration Backup

**Backup running config to Flash:**

```
copy run flash
```

**Backup startup config to TFTP:**

```
copy start tftp
```

---

### 6️⃣ IOS Image Backup

```
copy flash tftp
```

✔️ Backed up IOS image to TFTP server

---

### 7️⃣ IOS Recovery (ROMMON)

After deleting IOS:

```
delete flash:<image>
reload
```

Router enters ROMMON.

Recovery using:

```
IP_ADDRESS=10.10.10.1
IP_SUBNET_MASK=255.255.255.0
DEFAULT_GATEWAY=10.10.10.1
TFTP_SERVER=10.10.10.10
TFTP_FILE=c2900-universalk9-mz.SPA.151-4.M4.bin
tftpdnld
```

✔️ IOS restored via TFTP

---

### 8️⃣ IOS Upgrade (Switch)

```
show version
copy tftp flash
boot system c2960-lanbasek9-mz.150-2.SE4.bin
copy run start
reload
```

✔️ Upgraded from IOS 12.2 → 15.0

---

## 🔍 Verification Commands

```
show run
show start
show ip interface brief
show flash
show version
```

---

## 📸 Screenshots to Include

* Before factory reset (`show run`)
* After reset (empty config)
* ROMMON mode
* Password recovery step
* Interface down (before fix)
* Interface up (after fix)
* Flash backup (`show flash`)
* TFTP backup (server view)
* IOS backup to TFTP
* Boot failure (ROMMON prompt)
* IOS recovery (`tftpdnld`)
* Switch upgrade (before & after version)

---

## 🎯 Key Learnings

* Difference between **running-config and startup-config**
* Role of **config-register in boot process**
* How to recover devices using **ROMMON**
* Importance of **backups in networking**
* IOS upgrade and recovery procedures

---

## 📝 Notes

* Lab performed using Cisco Packet Tracer
* Filenames are case-sensitive in Cisco devices
* Always backup configs before making changes

---

