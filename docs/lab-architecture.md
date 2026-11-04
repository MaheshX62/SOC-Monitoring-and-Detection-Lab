# SOC Lab Architecture

## 1. Project Overview

This project implements a two-virtual-machine Security Operations Center (SOC) monitoring lab using Splunk Enterprise, Windows Security logs, Sysmon, and Splunk Universal Forwarder.

## 2. Lab Environment

| Component | Configuration |
|---|---|
| SIEM server | SOC-Splunk |
| SIEM operating system | Ubuntu Server 24.04.5 LTS |
| SIEM platform | Splunk Enterprise 10.6.0.5 |
| Endpoint | SOC-Windows |
| Endpoint operating system | Windows 11 Pro 26H2 |
| Endpoint telemetry | Windows Security and Sysmon |
| Log forwarding | Splunk Universal Forwarder |
| Splunk receiving port | TCP 9997 |
| Splunk Web | TCP 8000 |

## 3. Network Configuration

| Machine | Host-only IP |
|---|---|
| SOC-Windows | 192.168.56.101 |
| SOC-Splunk | 192.168.56.102 |

VirtualBox NAT networking provides internet connectivity. The Host-only network provides private communication between the two lab machines.

## 4. Data Collection

The Windows endpoint forwards the following telemetry to Splunk:

- Windows Security events.
- Sysmon Operational events.

Important event identifiers used in the project:

| Event ID | Description |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| Sysmon 1 | Process creation |
| Sysmon 3 | Network connection |

## 5. Detection Workflow

1. Generate controlled endpoint activity.
2. Record Windows Security and Sysmon events.
3. Forward events to Splunk.
4. Search and correlate events using SPL.
5. Identify suspicious patterns.
6. Investigate alerts and validate event context.
7. Document findings and map applicable behavior to MITRE ATT&CK.

## 6. Validation

The lab successfully ingested Windows Security and Sysmon telemetry.

A controlled failed-logon test generated Event ID 4625 records. A five-minute detection search identified two failed attempts for the SOC Analyst account from 127.0.0.1.

A scheduled Splunk alert was also triggered and its results were reviewed.

## 7. Security Scope

This environment is an isolated, authorized learning lab. Failed-logon activity was deliberately generated for detection testing. Results should not be interpreted as evidence of a real compromise.
