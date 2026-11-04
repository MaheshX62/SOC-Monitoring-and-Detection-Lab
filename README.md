# SOC Monitoring and Detection Lab

An end-to-end Security Operations Center (SOC) monitoring and detection lab built using Windows 11, Sysmon, Splunk Universal Forwarder, and Splunk Enterprise.

This project demonstrates endpoint telemetry collection, SIEM monitoring, detection engineering, alert validation, incident investigation, and MITRE ATT&CK mapping.

## 1. Project Objectives

- Build a two-VM SOC monitoring environment.
- Collect Windows Security and Sysmon telemetry.
- Forward endpoint logs to Splunk Enterprise.
- Develop SPL searches for authentication, process, and network activity.
- Detect repeated failed logon attempts.
- Configure and validate scheduled SIEM alerts.
- Investigate security events and document findings.
- Map relevant behavior to MITRE ATT&CK.

## 2. Lab Architecture

| Component | Configuration |
|---|---|
| SIEM server | SOC-Splunk |
| SIEM operating system | Ubuntu Server 24.04.5 LTS |
| SIEM platform | Splunk Enterprise 10.6.0.5 |
| Endpoint | SOC-Windows |
| Endpoint operating system | Windows 11 Pro 26H2 |
| Endpoint telemetry | Windows Security Logs and Sysmon |
| Log forwarding | Splunk Universal Forwarder |
| Splunk receiving port | TCP 9997 |
| Splunk Web | TCP 8000 |

### Network Configuration

| Machine | Host-only IP |
|---|---|
| SOC-Windows | 192.168.56.101 |
| SOC-Splunk | 192.168.56.102 |

VirtualBox NAT provides internet connectivity, while the Host-only network enables private communication between the lab machines.

## 3. Data Sources

| Source | Event ID | Description |
|---|---|---|
| Windows Security | 4624 | Successful logon |
| Windows Security | 4625 | Failed logon |
| Sysmon | 1 | Process creation |
| Sysmon | 3 | Network connection |

## 4. Detection Engineering

The repository contains these SPL searches:

| Detection file | Purpose |
|---|---|
| `detections/failed-logons.spl` | Detect repeated failed logons within five-minute windows |
| `detections/sysmon-process-creation.spl` | Summarize process creation activity |
| `detections/sysmon-network-connections.spl` | Summarize network connection activity |

### Failed Logon Detection

The primary detection searches for Windows Event ID 4625, extracts the failed target account, groups events into five-minute windows, and identifies groups containing at least two failed attempts.

A scheduled Splunk alert was configured to run every five minutes, using a last-five-minute search window. Its triggered results were successfully reviewed.

## 5. Controlled Security Scenario

Two intentionally incorrect password attempts were generated on the SOC-Windows endpoint.

### Observed Results

| Attribute | Result |
|---|---|
| Event ID | 4625 |
| Target account | SOC Analyst |
| Source address | 127.0.0.1 |
| Workstation | SOC-WINDOWS |
| Logon type | 2 |
| Failed attempts | 2 |
| Detection window | Five minutes |

The detection identified the two failed attempts, and the scheduled alert successfully triggered.

**Assessment:** This was authorized lab activity, not evidence of an actual compromise.

## 6. Investigation and MITRE ATT&CK

The repeated failed-logon behavior is relevant to:

- Tactic: Credential Access
- Technique: T1110 - Brute Force
- Candidate sub-technique: T1110.001 - Password Guessing

This is a behavioral mapping, not proof of malicious activity.

The IOC investigation covered:

- IP: `192.0.2.10`
- Domain: `ioc-test.example`

No matching telemetry was found for these test indicators. The observed Notepad executable hash did not receive a malicious or clean verdict.

## 7. Repository Structure

```text
SOC-Monitoring-and-Detection-Lab/
├── README.md
├── docs/
│   ├── incident-report.md
│   ├── lab-architecture.md
│   └── mitre-attack-mapping.md
├── detections/
│   ├── failed-logons.spl
│   ├── sysmon-process-creation.spl
│   └── sysmon-network-connections.spl
├── dashboards/
├── evidence/
│   ├── sanitized/
│   └── screenshots/
└── scripts/
```

## 8. Reproducibility Overview

1. Create two VirtualBox VMs: a Windows endpoint and an Ubuntu SIEM server.
2. Configure NAT and Host-only networking.
3. Install Splunk Enterprise on the Ubuntu server.
4. Enable the Splunk receiving port TCP 9997.
5. Install Sysmon and Splunk Universal Forwarder on the Windows endpoint.
6. Configure forwarding of Windows Security and Sysmon Operational logs.
7. Confirm ingestion in Splunk.
8. Run the SPL searches in the `detections/` directory.
9. Generate controlled failed-logon events in the authorized lab.
10. Review detection results and scheduled alert activity.

See `docs/lab-architecture.md` for environment details and `docs/incident-report.md` for investigation findings.

## 9. Limitations

- The failed-logon rule is threshold-based and does not independently establish malicious intent.
- Source address 127.0.0.1 indicates local endpoint activity.
- Process creation and network connection searches are monitoring summaries, not standalone malicious-activity verdicts.
- IOC searches with no matches do not prove an indicator is safe.
- This lab is intended for controlled educational and defensive security testing.

## 10. Skills Demonstrated

- SOC operations fundamentals
- Windows event log analysis
- Sysmon telemetry monitoring
- Splunk Enterprise and SPL
- Detection engineering
- Scheduled alert configuration
- Incident investigation
- IOC analysis
- MITRE ATT&CK mapping
- VirtualBox networking
- Technical documentation

## Author

Mahesh Yaramala

Cybersecurity | SOC Monitoring | SIEM | Detection Engineering

---

**Disclaimer:** This project was developed in an authorized virtual lab for educational and defensive cybersecurity purposes.