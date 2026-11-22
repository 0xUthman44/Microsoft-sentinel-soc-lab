# Telemetry and data collection

## Data sources

| Channel / provider | Event ID | Use in this lab |
|---|---:|---|
| Security | 4624 | Successful logons |
| Security | 4625 | Failed logons |
| Security | 4688 | Process creation |
| Security | 4698 | Scheduled task creation |
| Microsoft-Windows-PowerShell/Operational | 4104 | PowerShell script block logging |
| Microsoft-Windows-Sysmon/Operational | 3 | Network connections |
| Microsoft-Windows-Sysmon/Operational | 13 | Registry value set |

The events were queried in the Log Analytics `Event` table. The project did not use the `SecurityEvent` table for these investigations.

## Collection approach

AMA was installed on `ENT-WKS-01` and associated with `dcr-sentinel-soc-lab-security`. The DCR delivered selected event data to `law-sentinel-soc-lab`.

The collection configuration was expanded only when a scenario required additional telemetry:
- Sysmon Event ID 13 for Run Key value changes.
- Sysmon Event ID 3 for network connection telemetry.
- Security Event IDs 4624 and 4625 for successful and failed logon investigation.

The Security collection filter included the relevant process/task events already used in the project, including 4688 and 4698, and was extended to include 4624 and 4625.

## Why scoped collection matters

Windows Security and Sysmon channels can be high-volume. A portfolio lab should collect the minimum event set needed for the investigation, validate ingestion, and monitor workspace usage. Do not add broad wildcard collection or additional channels without evaluating expected volume and cost.

## Verification pattern

For each scenario:
1. Confirm the local event or logging source generated the expected record.
2. Query the workspace for the matching computer, event channel and ID.
3. Inspect the rendered event description and extract relevant fields.
4. Validate the query result against the known test action.
5. Capture a screenshot only when it adds meaningful evidence.

## Cost and account controls

The lab was designed around the available Azure free-trial credit and a preference to avoid unexpected charges. This documentation does not make a cost guarantee. Azure pricing and free allowances vary by region, agreement, service configuration, ingestion and retention. Review current billing and workspace usage before extending collection or leaving resources running.
