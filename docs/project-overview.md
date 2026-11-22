# Project overview

## Purpose

This project simulates selected Level 1/Level 2 SOC workflows using Microsoft Sentinel, Log Analytics, Azure Monitor Agent, Windows Security auditing, PowerShell script-block logging and Sysmon. It focuses on collecting endpoint telemetry, building KQL investigations and explaining what the evidence does—and does not—show.

## Objectives

- Establish a monitored Windows endpoint connected to a Sentinel-backed Log Analytics workspace.
- Collect a deliberately limited set of security-relevant Windows events.
- Validate telemetry arrival before writing detections.
- Investigate execution, discovery, persistence, network and authentication behaviours.
- Preserve evidence through query results and screenshots.
- Document the limits of attribution and the distinction between a simulation and a real incident.

## Environment

- Workspace: `law-sentinel-soc-lab`
- Resource group: `rg-sentinel-soc-lab`
- Region: East US
- Primary endpoint: `ENT-WKS-01` (Windows 10 Pro, build 19045)
- AMA: version 1.47.1.0
- DCR: `dcr-sentinel-soc-lab-security`
- Supporting system: `SENTINEL-SRV01` (Windows Server 2022 Standard Evaluation, connected through Azure Arc)

The Windows 10 endpoint was the primary telemetry source. The supporting server was used to originate the controlled remote SMB authentication test. Host IP addresses are intentionally generalised or redacted.

## Workflow

1. Prepare the endpoint and verify the relevant Windows services and logging sources.
2. Configure AMA and the DCR to collect the selected telemetry.
3. Generate one controlled behaviour at a time.
4. Verify the corresponding event locally where applicable.
5. Confirm that the event arrived in the Log Analytics `Event` table.
6. Build and validate KQL queries against the ingested data.
7. Capture screenshots of meaningful detection and investigation results.
8. Remove temporary test artefacts after evidence capture.
9. Document findings and limitations.

## Scenario results

| Scenario | Evidence source | Result |
|---|---|---|
| [PowerShell execution and discovery](scenarios/01-powershell-execution-and-discovery.md) | PowerShell 4104 | Script-block activity for discovery commands was observed and queried |
| [Registry Run Key persistence](scenarios/02-registry-runkey-persistence.md) | Sysmon 13 | Controlled Run Key value creation was detected |
| [Scheduled Task persistence](scenarios/03-scheduled-task-persistence.md) | Security 4698, Security 4688, PowerShell 4104 | Creation and execution were correlated in a timeline |
| [Outbound network activity](scenarios/04-outbound-network-investigation.md) | Sysmon 3 | PowerShell TCP connection to `example.com:443` was detected |
| [Authentication](scenarios/05-authentication-investigation.md) | Security 4624 and 4625 | Successful logons and controlled remote SMB failures were investigated |

## What this project demonstrates

- Endpoint telemetry onboarding and verification.
- Practical KQL filtering, extraction, projection and sorting.
- Windows logon event interpretation.
- Sysmon registry and network telemetry analysis.
- Correlation of multiple event types around a controlled test window.
- Careful interpretation of evidence, including acknowledging when attribution is inconclusive.

## Scope boundaries

This is not a full enterprise SOC deployment. It does not claim complete ATT&CK coverage, production-grade alert tuning, automated response, threat-intelligence enrichment, or comprehensive endpoint protection. The scenarios are controlled simulations intended to demonstrate investigation skills.
