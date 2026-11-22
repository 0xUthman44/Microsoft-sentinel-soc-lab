# Microsoft Sentinel SOC Lab

A low-cost Microsoft Sentinel portfolio lab covering endpoint telemetry onboarding, KQL investigation and event correlation across five controlled security scenarios.

> **Note:** Every scenario in this repository is a controlled simulation run in an isolated lab. The outbound network test and the failed logons were generated deliberately for validation. None of the activity represents a real compromise.

## What this project demonstrates

- Onboarding a Windows 10 endpoint to Microsoft Sentinel and a Log Analytics workspace using Azure Monitor Agent (AMA) and a Data Collection Rule (DCR).
- Collecting a deliberately scoped set of Windows Security, PowerShell and Sysmon events.
- Writing and validating KQL against the Log Analytics `Event` table.
- Correlating process, task and script-block events into an investigation timeline.
- Interpreting evidence carefully, including stating where attribution is inconclusive.

## Scenarios

| # | Scenario | Telemetry | Report | Query |
|---|---|---|---|---|
| 1 | PowerShell execution and host/account discovery | PowerShell 4104 | [Report](docs/scenarios/01-powershell-execution-and-discovery.md) | [`powershell-discovery.kql`](detections/powershell-discovery.kql), [categorised variant](detections/powershell-discovery-categorised.kql) |
| 2 | Registry Run Key persistence | Sysmon 13 | [Report](docs/scenarios/02-registry-runkey-persistence.md) | [`registry-runkey-persistence.kql`](detections/registry-runkey-persistence.kql) |
| 3 | Scheduled Task persistence (`WindowsUpdateCheckSimulation`) | Security 4698, 4688; PowerShell 4104 | [Report](docs/scenarios/03-scheduled-task-persistence.md) | [`scheduled-task-correlation.kql`](detections/scheduled-task-correlation.kql) |
| 4 | PowerShell outbound network activity (benign test) | Sysmon 3 | [Report](docs/scenarios/04-outbound-network-investigation.md) | [`powershell-outbound-network.kql`](detections/powershell-outbound-network.kql) |
| 5 | Authentication investigation, including controlled remote SMB failures | Security 4624, 4625 | [Report](docs/scenarios/05-authentication-investigation.md) | [`authentication-investigation.kql`](detections/authentication-investigation.kql) |

A consolidated write-up is in [`docs/investigation-summary.md`](docs/investigation-summary.md).

## Lab environment

| Component | Configuration |
|---|---|
| SIEM | Microsoft Sentinel |
| Log Analytics workspace | `law-sentinel-soc-lab` |
| Resource group | `rg-sentinel-soc-lab` |
| Azure region | East US |
| Primary monitored endpoint | `ENT-WKS-01` (Windows 10 Pro, build 19045) |
| Telemetry agent | Azure Monitor Agent (AMA) 1.47.1.0 |
| Data Collection Rule | `dcr-sentinel-soc-lab-security` |
| Supporting host | `SENTINEL-SRV01` (Windows Server 2022 Standard Evaluation, Azure Arc) |
| Query language | Kusto Query Language (KQL) |

Private and public IP addresses are intentionally omitted or redacted throughout. Details are in [`docs/lab-architecture.md`](docs/lab-architecture.md).

## Telemetry

All investigations query the Log Analytics **`Event`** table (not `SecurityEvent`).

| Source | Event ID | Purpose |
|---|---:|---|
| Windows Security | 4624 | Successful logon |
| Windows Security | 4625 | Failed logon |
| Windows Security | 4688 | Process creation |
| Windows Security | 4698 | Scheduled task created |
| PowerShell Operational | 4104 | Script block logging |
| Sysmon Operational | 3 | Network connection |
| Sysmon Operational | 13 | Registry value set |

Collection was scoped to these IDs and expanded in stages as each scenario required. See [`docs/telemetry-and-data-collection.md`](docs/telemetry-and-data-collection.md).

## Key outcomes

- Confirmed PowerShell script-block telemetry for controlled discovery commands.
- Detected a Sysmon registry value-set event for a controlled Run Key test.
- Correlated scheduled-task creation and execution using Security and PowerShell events.
- Detected a PowerShell-initiated TCP connection on port 443 from a benign connectivity test to `example.com`.
- Reviewed successful and failed logons, then generated three intentional remote SMB failures and verified the matching Security 4625 events.

## Repository layout

```text
microsoft-sentinel-soc-lab/
├── README.md
├── LICENSE
├── docs/
│   ├── project-overview.md
│   ├── lab-architecture.md
│   ├── telemetry-and-data-collection.md
│   ├── investigation-summary.md
│   ├── limitations-and-cost-controls.md
│   └── scenarios/
│       ├── 01-powershell-execution-and-discovery.md
│       ├── 02-registry-runkey-persistence.md
│       ├── 03-scheduled-task-persistence.md
│       ├── 04-outbound-network-investigation.md
│       └── 05-authentication-investigation.md
├── detections/
│   ├── README.md
│   └── *.kql
└── screenshots/
    └── detections/
```

## Where to start

Read [`docs/project-overview.md`](docs/project-overview.md), then the architecture and telemetry notes, then each scenario report. Each report covers the objective, controlled activity, telemetry, detection logic, result, interpretation and cleanup.

## Limitations and cost

This is not a production deployment guide and makes no claim of detection coverage. No cost figures are stated; Azure pricing varies by region, agreement, ingestion and retention. See [`docs/limitations-and-cost-controls.md`](docs/limitations-and-cost-controls.md).

## Disclaimer

Educational SOC lab. All tests were controlled and authorised within the lab environment.
