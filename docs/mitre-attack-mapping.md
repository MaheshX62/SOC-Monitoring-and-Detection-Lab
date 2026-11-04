# MITRE ATT&CK Mapping

## Detection: Repeated Failed Logons

| Attribute | Mapping |
|---|---|
| Data source | Windows Security Event Log |
| Event ID | 4625 |
| Tactic | Credential Access |
| Technique | T1110 - Brute Force |
| Sub-technique | T1110.001 - Password Guessing |
| Detection behavior | Multiple failed logons within five minutes |

## Interpretation

Repeated failed logons can be associated with password-guessing behavior.

The mapping is based on observed behavior, not proof of malicious intent. In this project, the events were deliberately generated as an authorized detection test.

## Other Telemetry

| Telemetry | Event ID | ATT&CK mapping |
|---|---|---|
| Notepad process creation | Sysmon 1 | No malicious technique assigned |
| Network connection telemetry | Sysmon 3 | No technique assigned without additional suspicious context |

## Detection Limitation

A threshold-based failed-logon detection can identify repeated authentication failures, but it cannot independently distinguish legitimate mistakes, authorized testing, and malicious activity.
