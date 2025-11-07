# 🔐 pfSense OpenVPN Configuration Guide

## 📘 Overview
This guide covers the setup of **OpenVPN on pfSense** for secure remote access to internal network resources.

The VPN will allow authorized users (admins, analysts, or remote employees) to connect to the LAN/VLANs securely from outside the network.

---

## 🧩 Network Context
| Component | Description |
|------------|--------------|
| VPN Type | Remote Access (SSL/TLS) |
| VPN Subnet | 10.50.50.0/24 |
| Access Scope | VLAN10 + VLAN20 |
| Authentication | Local User Certificates |

---

## ⚙️ Configuration Steps

### 1️⃣ Create a Certificate Authority
1. Go to **System > Cert. Manager > CAs**
2. Click **Add**
3. Fill details → Save (Name: `OpenVPN-CA`)

### 2️⃣ Create Server Certificate
1. **System > Cert. Manager > Certificates**
2. Add → Create Internal Certificate
   - Descriptive name: `OpenVPN-Server`
   - Common Name: `vpn.yourdomain.local`

### 3️⃣ OpenVPN Wizard
1. Go to **VPN > OpenVPN > Wizards**
2. Select existing CA (`OpenVPN-CA`)
3. Choose Server Mode: **Remote Access (SSL/TLS)**
4. Set tunnel network: `10.8.0.0/24`
5. Redirect Gateway: ✅ Enabled (for full tunnel)
6. Save & Apply

### 4️⃣ Firewall & NAT Rules
| Interface | Source | Destination | Protocol | Action |
|------------|----------|-------------|-----------|----------|
| WAN | any | WAN Address | UDP 1194 | ✅ Allow |
| OpenVPN | OpenVPN net | any | any | ✅ Allow |

### 5️⃣ Client Export
1. Navigate to **VPN > OpenVPN > Client Export**
2. Select a user, download `.ovpn` file
3. Import into OpenVPN GUI on client

---

## 🧠 Verification
- Connect from an external host.
- Check client IP (`10.8.0.x`)
- Ping LAN/VLAN hosts for access validation.

---

## 🧾 License
MIT License — for educational & cybersecurity research use.

**Author:** Vutlhari Mathebula  
Focus: VPN Security • Remote Access • Network Encryption
