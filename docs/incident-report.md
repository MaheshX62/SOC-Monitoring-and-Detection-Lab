# Incident Investigation Report

## 1. Incident Overview

| Field | Details |
|---|---|
| Incident type | Repeated failed logon attempts |
| Environment | Authorized SOC lab |
| Endpoint | SOC-Windows |
| SIEM | Splunk Enterprise |
| Windows Event ID | 4625 |
| Severity | Informational / Low |
| Investigation outcome | Controlled test activity confirmed |

## 2. Detection Details

Two failed interactive logon events were generated intentionally on the SOC-Windows endpoint.

| Field | Observed value |
|---|---|
| Target account | SOC Analyst |
| Subject account | SOC-WINDOWS$ |
| Source address | 127.0.0.1 |
| Workstation | SOC-WINDOWS |
| Logon type | 2 |
| Status | 0xC000006D |
| SubStatus | 0xC000006A |
| First event | 2026-10-19 03:35:30.383 UTC |
| Second event | 2026-10-19 03:35:34.532 UTC |
| Time difference | 4.149 seconds |

## 3. Investigation Process

1. Searched Windows Security telemetry in Splunk.
2. Filtered for Event ID 4625.
3. Reviewed account, workstation, source address, and logon type.
4. Extracted the failed target account from the event message.
5. Grouped events into five-minute windows.
6. Confirmed that the detection identified the two failed attempts.

## 4. Detection Logic

The detection groups failed logons into five-minute windows and reports groups containing at least two failures.

See `../detections/failed-logons.spl`.

## 5. Findings

- Two failed logon events were observed.
- Both events targeted the SOC Analyst account.
- Both originated from the local endpoint address 127.0.0.1.
- Both were interactive logon attempts.
- The activity was intentionally generated during authorized lab testing.

## 6. MITRE ATT&CK Context

The behavior is relevant to Credential Access technique T1110 (Brute Force). Password Guessing, T1110.001, is a possible behavioral mapping.

This mapping describes the observed behavior and does not establish that an actual attacker was present.

## 7. Conclusion

The Splunk detection successfully identified the controlled failed-logon activity. The investigation verified event context and demonstrated an end-to-end detection workflow.

No evidence of an actual compromise was established by this test.
