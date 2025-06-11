# 🔐 Detect and Respond: Windows Server RDP Brute Force Attack

## 📌 Description
This project simulates a **Remote Desktop Protocol (RDP) brute-force attack** using Hydra from a Kali Linux machine targeting a Windows Server 2019 instance. The objective is to detect unauthorized access attempts using Windows Event Viewer and respond with appropriate incident response steps.

---

## 🧰 Lab Setup

### 🖥️ Machines Required
- **Windows Server 2019/2022**
  - RDP Enabled
  - Event Viewer Access
  - Test user account created (`attackerlab`)
- **Kali Linux**
  - Hydra pre-installed
- **Network**
  - Both machines connected to the same LAN/Virtual Network
  - RDP Port 3389 allowed through firewall

---

## ⚙️ Windows Server Preparation

```powershell
# Enable RDP
Control Panel → System → Remote Settings → Enable Remote Desktop

# Allow RDP in Windows Firewall
Windows Defender Firewall → Inbound Rules → Enable "Remote Desktop (TCP-In)"

# Create Test User
net user attackerlab Password123 /add

# Install Hydra
sudo apt update && sudo apt install hydra

# Run Brute Force Attack
hydra -t 4 -V -f -l attackerlab -P /usr/share/wordlists/rockyou.txt rdp://<Windows_Server_IP>


# Disable the compromised user account
net user attackerlab /active:no

# Block attacker's IP address via PowerShell
New-NetFirewallRule -DisplayName "Block Attacker" -Direction Inbound -RemoteAddress <Kali_IP> -Action Block
