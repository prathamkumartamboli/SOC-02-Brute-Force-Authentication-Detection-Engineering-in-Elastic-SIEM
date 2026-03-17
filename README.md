# 🔐 SOC-02: Brute Force Authentication Detection in Elastic SIEM

## 📌 Project Overview
This project demonstrates the detection of brute-force authentication attacks using Elastic SIEM in a controlled SOC lab environment.

A simulated attacker performed multiple failed login attempts against a Windows system, generating Event ID 4625 logs. These logs were analyzed to create a behavior-based detection rule.

---

## 🎯 Objective
- Simulate brute-force authentication attack
- Capture authentication logs (Event ID 4625)
- Forward logs using Winlogbeat
- Detect attack behavior in Elastic SIEM
- Investigate alerts using Timeline
- Perform incident response

---

## 🧠 Attack Overview
A brute-force attack involves repeated login attempts using different passwords.

### Key Indicators:
- Multiple failed login attempts
- Same source IP
- Same user targeted
- High frequency in short time

---

## 🏗️ Lab Architecture
Kali Linux (Attacker)
        │
        │  Brute Force Attempts
        ▼
Windows 11 (Target + SIEM Host)
        │
        │  Winlogbeat
        ▼
Elasticsearch → Kibana

---

## 🛠️ Tools & Technologies
- Kali Linux (Attacker)
- SMBClient / Hydra (Attack Simulation)
- Windows 11 (Target)
- Sysmon (Logging)
- Winlogbeat (Log Forwarding)
- Elastic Stack (SIEM)

---

## 📊 Log Source
### Windows Security Event ID 4625
Represents failed login attempts.

Key Fields:
- source.ip
- user.name
- event.code
- host.name
- @timestamp

---

## ⚔️ Attack Simulation

### Step 1: Connectivity Check
ping 192.168.76.100

### Step 2: Username Enumeration
enum4linux 192.168.76.100

### Step 3: Brute Force Attempt
smbclient -L //192.168.76.100 -U Pratham

Each failed attempt generates:
Event ID 4625 – Failed Login

---

## 📈 Detection Logic

### Detection Query
winlog.channel:Security AND event.code:4625

### Threshold Rule
- Group by: source.ip
- Threshold: > 5 events
- Time window: 1 minute

---

## 🚨 Alert Example
source.ip: 192.168.76.101  
user.name: Pratham  
event.code: 4625  
attempts: 8  

---

## 🔎 Investigation Process
1. Identify repeated login failures  
2. Confirm attacker IP  
3. Analyze targeted user  
4. Review timeline  
5. Check for successful login (Event ID 4624)  

---

## 📌 Indicators of Compromise (IOCs)
- Source IP: 192.168.76.101  
- Target IP: 192.168.76.100  
- Event ID: 4625  
- Username: Pratham  
- Protocol: SMB  

---

## 📉 Risk Assessment
- Threat Stage: Credential Access  
- Severity: Medium  
- Impact: Low  
- Compromise: No  

---

## 🛡️ Mitigation

### Block Attacker IP
netsh advfirewall firewall add rule name="Block Brute Force Attacker" dir=in action=block remoteip=192.168.76.101

---

## 🔧 Detection Tuning
winlog.channel:Security AND event.code:4625 AND source.ip:* AND NOT source.ip:127.0.0.1 AND NOT user.name:"SYSTEM"

---

## 🧩 MITRE ATT&CK Mapping
- Tactic: Credential Access  
- Technique: Brute Force  
- ID: T1110  
- Sub-Technique: Password Guessing  

---

## 📚 Conclusion
This project demonstrates how SOC teams detect, analyze, and respond to brute-force authentication attacks using Elastic SIEM.

---

## 👨‍💻 Author
Pratham Tamboli
