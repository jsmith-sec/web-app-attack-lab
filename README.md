<div align="center">

# 🚨 Incident Response Lab — IR-001
### SSH Compromise → Containment → Recovery

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3500&pause=800&color=2F81F7&center=true&vCenter=true&width=690&lines=Full-Lifecycle+Incident+Response+%7C+PICERL;Brute+Force+to+Backdoor+to+Recovery;Contained+%26+Recovered+in+~20+Minutes" alt="typing summary" />

<p>
  <img src="https://img.shields.io/badge/Type-Incident%20Response%20%2F%20Blue%20Team-0A2A66?style=for-the-badge" alt="type" />
  <img src="https://img.shields.io/badge/Mapped%20to-MITRE%20ATT%26CK-2F81F7?style=for-the-badge" alt="mitre" />
  <a href="ir_report.pdf"><img src="https://img.shields.io/badge/Full%20Report-PDF-0A2A66?style=for-the-badge&logo=adobeacrobatreader&logoColor=white" alt="full report" /></a>
</p>

<p>
  <img src="https://img.shields.io/badge/Elastic%20Stack-005571?style=flat-square&logo=elasticstack&logoColor=white" alt="elastic" />
  <img src="https://img.shields.io/badge/Kibana-005571?style=flat-square&logo=kibana&logoColor=white" alt="kibana" />
  <img src="https://img.shields.io/badge/Ubuntu%2026.04%20ARM64-E95420?style=flat-square&logo=ubuntu&logoColor=white" alt="ubuntu" />
  <img src="https://img.shields.io/badge/Kali%20Linux-557C94?style=flat-square&logo=kalilinux&logoColor=white" alt="kali" />
  <img src="https://img.shields.io/badge/Nmap-2F81F7?style=flat-square" alt="nmap" />
  <img src="https://img.shields.io/badge/Hydra-2F81F7?style=flat-square" alt="hydra" />
  <img src="https://img.shields.io/badge/UFW%20Firewall-2F81F7?style=flat-square" alt="ufw" />
</p>

</div>

A hands-on incident response simulation conducted on a home lab environment. An attacker machine (Kali Linux) compromised an Ubuntu target via SSH brute force, performed post-compromise reconnaissance, created backdoor accounts with sudo privileges, and planted a persistence mechanism. The full PICERL incident response methodology was followed to contain, eradicate, and recover from the incident.

## Environment

| Component | Details |
|---|---|
| SIEM Host | Ubuntu 26.04 ARM64 — Elasticsearch, Kibana, Elastic Agent |
| Attacker | Kali Linux 2023 ARM64 |
| Host Machine | Apple Mac Mini M4, 32GB RAM |
| Network | Bridged (192.168.1.0/24) |

## Attack Summary

| Phase | Technique | MITRE ID |
|---|---|---|
| Reconnaissance | Nmap SYN scan | T1046 |
| Credential Access | SSH brute force via Hydra | T1110 |
| Discovery | whoami, id, /etc/passwd, ps aux | T1033, T1087, T1057 |
| Persistence | Backdoor accounts created with sudo access | T1136 |
| Persistence | Malicious cron job planted | T1053 |
| Privilege Escalation | Backdoor accounts added to sudo group | T1078 |

## Detection

Three detection rules fired during the incident:

| Rule | Type | Severity |
|---|---|---|
| SSH Brute Force Detection | Custom threshold rule | Medium |
| Potential Internal Linux SSH Brute Force Detected | Elastic prebuilt | Medium |
| Suspicious Account Creation or Modification | Custom threshold rule | High |

851 failed authentication attempts captured. All attack phases logged and alerted on in real time through the ELK stack SIEM.

## Kibana Alert Timeline

![Kibana Alerts](alerts%202.png)

## IR Response (PICERL)

- **Preparation** — System baseline captured before incident
- **Identification** — Kibana alerts triggered investigation. Active attacker session confirmed via last and who commands
- **Containment** — Attacker IP blocked at firewall. Backdoor accounts locked
- **Eradication** — Backdoor accounts deleted. Malicious cron job removed. Compromised password reset
- **Recovery** — Firewall hardened with default deny policy. SSH restricted to admin IP only. Services verified operational
- **Lessons Learned** — Root cause identified as weak password + no host firewall. 7 hardening recommendations documented

## Response Time

~20 minutes from identification to recovery

## Full Report

See [ir_report.pdf](ir_report.pdf) for the complete incident response report including full timeline, forensic evidence, MITRE ATT&CK mapping, and recommendations.

## Tools Used

- Elastic Stack 8.19.14 (Elasticsearch, Kibana, Elastic Agent)
- Kali Linux — Nmap, Hydra
- UFW (host-based firewall)
- Ubuntu 26.04 ARM64

## Other Labs in This Series

| Lab | Topic | Repo |
|---|---|---|
| Lab 1 | SOC/SIEM Detection | [soc-siem-lab](https://github.com/jsmith-sec/soc-siem-lab) |
| Lab 2 | Incident Response Simulation | This repo |
| Lab 3 | Web Application Attack | [web-app-attack-lab](https://github.com/jsmith-sec/web-app-attack-lab) |
| Lab 4 | Vulnerability Assessment | [vulnerability-assessment-lab](https://github.com/jsmith-sec/vulnerability-assessment-lab) |
| Lab 5 | Malware Analysis | [malware-analysis-lab](https://github.com/jsmith-sec/malware-analysis-lab) |
| Lab 6 | Phishing Analysis | [phishing-analysis-lab](https://github.com/jsmith-sec/phishing-analysis-lab) |
| Lab 7 | Active Directory Attack | [active-directory-lab](https://github.com/jsmith-sec/active-directory-lab) |
