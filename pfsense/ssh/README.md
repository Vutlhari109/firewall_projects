# 🧰 pfSense Secure SSH Access Configuration

## 📘 Overview
This document explains how to enable and secure **SSH access** to pfSense for administrative control and troubleshooting.

SSH allows remote shell management — but must be hardened to prevent unauthorized access.

---

## ⚙️ Enable SSH Access

1. Navigate to **System > Advanced > Admin Access**
2. Under **Secure Shell**, check ✅ “Enable Secure Shell”
3. Select port: `22` (or change to a custom port, e.g. `2222`)
4. Save and Apply

---

## 🧱 SSH Firewall Rule
| Interface | Source | Destination | Protocol | Action | Description |
|------------|----------|-------------|-----------|----------|--------------|
| WAN | Admin IP | WAN Address | TCP 2222 | ✅ Allow | Allow admin SSH |
| WAN | any | WAN Address | TCP 22 | ❌ Block | Block default SSH port |

> 💡 Always restrict SSH to **specific admin IPs or VLAN20**.

---

## 🔐 Hardening SSH
1. Use **key-based authentication** (disable passwords)
   - Upload public key under: `System > User Manager > User > Authorized Keys`
2. Disable `root` login
3. Set SSH timeout (e.g., 60 seconds)
4. Enable 2FA for admin accounts if possible

---

## 🧩 Access via Terminal
```bash
ssh  admin@10.10.10.1

