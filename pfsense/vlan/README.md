# 🧱 pfSense VLAN Configuration Project

## 📘 Overview
This document explains how to configure **VLANs (Virtual Local Area Networks)** on pfSense to segment network traffic and enforce security boundaries.

VLANs are essential for isolating departments or devices in enterprise or lab networks.  
In this project, we’ll configure VLANs for:
- Students (VLAN10)
- Administrators / SOC Analysts (VLAN20)

---

## 🧩 Network Setup

| VLAN | Interface | Subnet | Description |
|------|------------|---------|--------------|
| VLAN10 | em1.10 | 10.30.10.0/24 | Students / Users |
| VLAN20 | em1.20 | 10.30.20.0/24 | Admins / SOC Analysts |

---

## ⚙️ Configuration Steps

### 1️⃣ Create VLAN Interfaces
1. Navigate to **Interfaces > Assignments > VLANs**
2. Click **Add**
   - Parent Interface: `em1`
   - VLAN Tag: `10`
   - Description: `VLAN10_Students`
3. Repeat for VLAN20 (`em1`, VLAN Tag: `20`)

### 2️⃣ Assign VLANs to Interfaces
1. Go to **Interfaces > Assignments**
2. Add each new VLAN (`em1.10`, `em1.20`)
3. Enable and configure static IPs:
   - VLAN10 → `10.30.10.1/24`
   - VLAN20 → `10.30.20.1/24`

### 3️⃣ Enable DHCP per VLAN
- Navigate to **Services > DHCP Server**
- Select VLAN10, enable DHCP:
  - Range: `10.30.10.10 - 10.30.10.200`
- Repeat for VLAN20

### 4️⃣ Firewall Rules
| Interface | Source | Destination | Action | Description |
|------------|----------|-------------|----------|--------------|
| VLAN10 | VLAN10 net | any | ✅ Allow | Allow internet access |
| VLAN20 | VLAN20 net | any | ✅ Allow | Allow internet access |
| VLAN10 | VLAN10 net | VLAN20 net | ❌ Block | Prevent cross-traffic |

> 💡 Test VLAN separation using `ping` between VLAN clients — traffic should be blocked.

---

## 🔍 Verification
- Use Wireshark or `traceroute` to ensure isolation.
- Check DHCP leases for VLAN clients.
- Test inter-VLAN blocking by attempting to reach another subnet.

---

## 🧾 License
MIT License — for educational & cybersecurity lab simulation.

**Author:** Vutlhari Mathebula  
Focus: Network Defense • VLAN Security • Firewall Segmentation
