# 🧱 pfSense Firewall Configuration & Network Security Project

## 📘 Overview
This project documents the setup and configuration of a **pfSense Firewall** — an open-source, FreeBSD-based firewall and router platform.  
It is designed for **network segmentation, intrusion detection, VPN access, and centralized log analysis**.

The goal is to simulate a **real-world enterprise network** with:
- Secure LAN, DMZ, and VLANs
- VPN for remote access
- IDS/IPS threat detection (Snort/Suricata)
- Centralized logging using **Splunk** or **ELK Stack**
- Security monitoring and rule-based traffic control

---

## 🧩 Project Architecture

### 🔹 Network Topology
| Network Zone | Interface | Subnet | Purpose |
|---------------|------------|--------|----------|
| WAN | em0 | DHCP / Static | Internet connection |
| LAN | em1 | 10.10.10.0/24 | Internal trusted network |
| DMZ | em2 | 10.20.20.0/24 | Public services (Web, Mail, etc.) |
| VLAN10 | em1.10 | 10.30.10.0/24 | Students / Users |
| VLAN20 | em1.20 | 10.30.20.0/24 | Admins / SOC Analysts |

---

## ⚙️ Installation

### Requirements
- VirtualBox / VMware / Bare Metal Server  
- pfSense ISO (latest version)  
- 2+ Network Interfaces  
- 2 GB RAM (minimum)  
- 10 GB Disk Space  

### Steps
1. Download pfSense ISO from [https://www.pfsense.org/download/](https://www.pfsense.org/download/).
2. Create a VM with 2 NICs:
   - **Adapter 1:** Bridged (WAN)
   - **Adapter 2:** Internal Network (LAN)
3. Boot from the ISO and complete installation.
4. Access Web GUI via browser:
https://10.10.10.1

markdown
Copy code
5. Default login:
Username: admin
Password: pfsense

yaml
Copy code

---

## 🧠 Basic Configuration

### Set Interfaces
- Assign **WAN** and **LAN**.
- Enable DHCP Server on LAN.
- Add **VLANs** under `Interfaces > Assignments > VLANs`.

### DNS & NTP
- Configure system DNS servers (e.g., `8.8.8.8`, `1.1.1.1`).
- Set correct NTP time sync for log accuracy.

### NAT & Port Forwarding
- Go to `Firewall > NAT`.
- Add port forwarding rules for DMZ services (e.g., Web Server 80/443).

---

## 🧱 Firewall Rules

| Interface | Source | Destination | Protocol | Action | Description |
|------------|----------|-------------|-----------|----------|--------------|
| LAN | LAN net | any | any | ✅ Allow | Allow internal traffic |
| DMZ | any | DMZ server | TCP 80,443 | ✅ Allow | Public web access |
| WAN | any | LAN net | any | ❌ Block | Prevent inbound traffic |
| VLAN10 | VLAN10 net | VLAN20 net | any | ❌ Block | Isolate VLANs |

> 💡 Always place **block rules before allow rules**.

---

## 🔐 VPN Configuration (Optional)

### Remote Access OpenVPN
1. Navigate to `VPN > OpenVPN`.
2. Run the **OpenVPN Wizard**.
3. Create user certificates for clients.
4. Export client configuration via `VPN > OpenVPN > Client Export`.

---

## 🕵️ IDS/IPS Setup (Snort or Suricata)

### Installation
1. Navigate to `System > Package Manager > Available Packages`.
2. Install **Snort** or **Suricata**.
3. Enable on LAN/WAN interfaces.
4. Subscribe to free rule sets (Emerging Threats / Snort VRT).

### Recommended Settings
- Enable **IPS Mode**
- Enable **Auto-Block Offenders**
- Log to `/var/log/snort/` or `/var/log/suricata/`

---

## 📊 Log Forwarding (SIEM Integration)

### To Splunk
1. In pfSense, go to `Status > System Logs > Settings`.
2. Enable **Remote Syslog Server**.
3. Add Splunk server IP and port (default `514`).
4. On Splunk:
index = pfsense sourcetype = syslog

markdown
Copy code
5. Verify logs are appearing in Splunk search.

### To ELK (Elastic Stack)
1. Forward logs to elk using:
Remote Syslog: <elk_server_ip>:514

yaml
Copy code
2. Parse logs using `pfSense.conf` in Logstash.

---

## 🔍 Security Testing (Lab Use)
You can simulate attacks or traffic to test rules:
- **Nmap**: Scan from Kali → pfSense LAN  
- **Hydra / Nikto**: Brute-force DMZ web services  
- **Wireshark**: Capture and analyze dropped packets  

---

## 🧰 Troubleshooting

| Issue | Cause | Solution |
|--------|--------|----------|
| No internet from LAN | Missing NAT rule | Enable outbound NAT |
| pfSense not resolving DNS | DNS Resolver misconfigured | Enable DNS Resolver service |
| VPN not connecting | Wrong client cert or port | Check OpenVPN logs |
| Log forwarding failed | Syslog port closed | Open UDP/TCP 514 on SIEM |


---

## 🧾 License
This project is licensed under the **MIT License** — free to use, modify, and share for educational and cybersecurity research purposes.

---

## 👨‍💻 Author
**Vutlhari Mathebula**  
Cybersecurity & Network Defense Enthusiast  
🛰️ Focus: Firewalls • SIEM • Threat Detection • Network Forensics  
📧 Contact: vutlharimathebula74@gmail.com
