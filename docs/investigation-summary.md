# Investigation summary

## Executive summary

Five controlled investigation scenarios were completed using Microsoft Sentinel and endpoint telemetry from `ENT-WKS-01`. The work covered PowerShell execution and discovery, two Windows persistence mechanisms, outbound network telemetry, and authentication activity.

The project demonstrates a repeatable SOC workflow: generate controlled activity, verify telemetry, query and interpret the evidence, and record limitations.

## Findings by scenario

### 1. PowerShell execution and discovery

Report: [Scenario 01](scenarios/01-powershell-execution-and-discovery.md)

PowerShell Script Block Logging (Event ID 4104) recorded controlled host and account discovery commands, including identity, group membership, hostname, network configuration, system information and local account/group enumeration. The corresponding detection query was validated.

### 2. Registry Run Key persistence

Report: [Scenario 02](scenarios/02-registry-runkey-persistence.md)

A controlled value named `WindowsSecurityHealthMonitor` was created under the current user's `Software\Microsoft\Windows\CurrentVersion\Run` key. Sysmon Event ID 13 was ingested and detected. The value was a harmless simulation command and was removed after evidence capture.

### 3. Scheduled Task persistence

Report: [Scenario 03](scenarios/03-scheduled-task-persistence.md)

A controlled scheduled task named `WindowsUpdateCheck` was created as part of the `WindowsUpdateCheckSimulation`. Security Event ID 4698 confirmed task creation; Event ID 4688 and PowerShell Event ID 4104 provided execution-related evidence. The task ran a harmless command (`whoami.exe` and `hostname.exe`) and was removed after evidence collection. The available process evidence did not conclusively identify the creator process.

### 4. Outbound network activity

Report: [Scenario 04](scenarios/04-outbound-network-investigation.md)

Sysmon Event ID 3 captured a PowerShell-initiated TCP connection to `example.com` on port 443. The Sentinel query extracted the process, account, source, destination, port, protocol and initiation status. This was a benign connectivity test and must not be represented as malicious traffic.

### 5. Authentication investigation

Report: [Scenario 05](scenarios/05-authentication-investigation.md)

Security Event IDs 4624 and 4625 were used to review successful and failed logons. The results included successful `SYSTEM` service logons (logon type 5) and three deliberately generated remote SMB authentication failures from `SENTINEL-SRV01` against the `SOC` account on `ENT-WKS-01`. The failures used NTLM and network logon type 3. The endpoint recorded a source network address that may differ from the source host's configured interface address because of virtual networking; the recorded value is reported as observed.

## Overall result

All five investigation scenarios were completed and the corresponding KQL investigations were validated in the lab. Evidence screenshots are in `screenshots/detections/` and are embedded in each scenario report.

## Analyst takeaways

- A log event provides evidence of an action, not automatically proof of malicious intent.
- Correlating event IDs across channels provides stronger context than relying on a single record.
- Collection scope and ingestion cost must be considered when enabling additional telemetry.
- Field extraction depends on the event rendering format and should be checked against raw records.
- Attribution should remain qualified when the telemetry does not conclusively support it.
