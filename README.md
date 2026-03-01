# SOC Lab 04 — SSH Authentication Event Detection

## 📑 Table of Contents
1. [Executive Summary](#executive-summary)
2. [Lab Objectives](#lab-objectives)
3. [Environment Overview](#environment-overview)
4. [Operational Workflow](#operational-workflow)
5. [Threat Simulation](#threat-simulation)
6. [Log Acquisition & Analysis](#log-acquisition--analysis)
7. [Detection Engineering Insights](#detection-engineering-insights)
8. [Evidence](#evidence)
9. [Conclusions](#conclusions)
10. [Next Steps](#next-steps)

---

## Executive Summary
This lab demonstrates SOC analyst fundamentals by generating and analyzing SSH authentication activity on a Linux system.  
Both **successful** and **failed** SSH login attempts were intentionally produced to simulate attacker behavior.  
These events create logs that are essential for brute-force detection, threat hunting, and SIEM correlation.

All evidence is documented and stored in the `evidence/` directory.

---

## Lab Objectives
- Validate Ubuntu network connectivity  
- Trigger SSH authentication events from a Kali host  
- Analyze SSH logs using `journalctl`  
- Identify failed login and brute-force indicators  
- Document evidence using SOC-style structure  
- Push all work to GitHub using a fine-grained PAT  

---

## Environment Overview
**Target Host:** Ubuntu Linux  
**Attacker Host:** Kali Linux  
**Hypervisor:** VMware Workstation  

**Tools Used:**
- SSH client  
- `journalctl`  
- ICMP (`ping`)  
- Git + GitHub  
- Fine-grained Personal Access Token (PAT)

---

## Operational Workflow

### 1. Connectivity Validation
Ensured the Ubuntu VM had network access using ICMP:

```bash
ping -c 4 8.8.8.8
```

---

### 2. SSH Authentication Event Generation
Triggered authentication attempts from the Kali attacker VM:

- Valid user login  
- Invalid password attempts  
- Invalid user attempts  

This produces realistic authentication artifacts needed for SOC analysis.

---

### 3. Log Retrieval

Collected relevant logs using:

```bash
sudo journalctl -u ssh --no-pager | tail -20
```

---

## Threat Simulation
Simulated adversary-style activity by attempting:

- Incorrect passwords  
- Logins using invalid users  
- Repeated authentication attempts  

This produces high-value log signals and supports brute-force detection logic.

---

## Log Acquisition & Analysis

### Commands Executed

**Connectivity Test:**
```bash
ping -c 4 8.8.8.8
```

**SSH Log Review:**
```bash
sudo journalctl -u ssh --no-pager | tail -20
```

---

### Sample SSH Log Output

```text
Feb 20 10:55:09 ubuntu sshd[1432]: Failed password for invalid user testuser from 192.168.58.130 port 42318 ssh2
Feb 20 10:55:11 ubuntu sshd[1432]: PAM authentication failure; rhost=192.168.58.130
Feb 20 10:56:02 ubuntu sshd[1500]: Accepted password for eric from 192.168.58.130 port 42355 ssh2
```

---

### Key Observations
- Failed login attempts clearly logged (`Failed password`)  
- Invalid user enumeration visible  
- Accepted authentication logged with session details  
- Source IP recorded for all attempts  
- Clear timestamps allow timeline reconstruction  

These logs are essential for detecting brute-force activity or unauthorized access attempts.

---

## Detection Engineering Insights

SSH authentication logs support multiple SOC and SIEM use cases:

### 🚨 High-Value Alert Conditions
- Repeated failed SSH logins from the same source  
- Multiple username attempts from one IP  
- Failed → successful login sequences  
- Authentication attempts from abnormal IP ranges  
- Password guessing behavior over a short timeframe  

These logs can support:
- Detection rule development  
- Threat hunting queries  
- Incident triage  
- Behavioral anomaly detection  

This lab builds foundational detection capabilities.

---

## Evidence

All screenshots are stored in the `evidence/` directory:

- [Ping Connectivity Test](./evidence/lab01_ping_success.png)  
- [Failed SSH Authentication](./evidence/lab01_ssh_failed.png)  
- [SSH Journal Log Evidence](./evidence/lab01_ssh_journal_logs.png)  

These files document each major stage of the lab and validate all findings.

---

## Conclusions

- SSH authentication logs provide reliable visibility into unauthorized access attempts.  
- Failed login attempts are strong signals of brute-force or credential misuse.  
- This lab successfully captured, analyzed, and documented SSH authentication events.  
- The repository now reflects a professional SOC-style documentation structure suitable for job applications and portfolio use.  

---

## Next Steps

To continue developing SOC skills:

- **SOC Lab 02 — SSH Brute-Force Detection & SIEM Correlation**  
- Authentication anomaly playbook development  
- Linux `/var/log/auth.log` deep-dive  
- Sudo escalation and privilege misuse detection  
- Firewall log analysis (UFW, iptables)  
- SIEM ingestion + detection engineering labs  

This lab provides the foundation for more advanced SOC workflows.
