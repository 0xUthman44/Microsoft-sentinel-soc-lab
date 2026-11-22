# Lab architecture

## Logical design

```text
                   Azure
  ┌─────────────────────────────────────────┐
  │ Resource group: rg-sentinel-soc-lab      │
  │                                         │
  │  Log Analytics workspace                │
  │  law-sentinel-soc-lab                   │
  │            ▲                            │
  │            │                            │
  │  Microsoft Sentinel                     │
  │            ▲                            │
  │            │ AMA / DCR                  │
  └────────────┼────────────────────────────┘
               │
        ENT-WKS-01
   Windows 10 Pro endpoint
   Sysmon + PowerShell + Security logs
               ▲
               │ controlled SMB test
               │
       SENTINEL-SRV01
   Windows Server 2022
   Azure Arc-connected support host
```

## Components

### Microsoft Sentinel and Log Analytics

The workspace `law-sentinel-soc-lab` was the central destination for collected telemetry and KQL investigations. Microsoft Sentinel was enabled on the workspace.

### Primary endpoint: ENT-WKS-01

The Windows 10 Pro endpoint was the main source of the project's telemetry. Azure Monitor Agent (AMA) version 1.47.1.0 was installed and running. Sysmon and PowerShell logging provided additional visibility beyond the basic Windows Security events.

### Supporting host: SENTINEL-SRV01

The Windows Server 2022 system was used as a separate source for the controlled SMB authentication test. It was not the primary monitored endpoint in the project.

### Data Collection Rule

The DCR `dcr-sentinel-soc-lab-security` associated the endpoint with the workspace and controlled which event channels and IDs were collected. Collection was expanded in stages to support the scenarios. The project deliberately avoided collecting every possible event category.

## Network and privacy

The lab used private virtual-machine networks. This documentation omits host-specific private addresses, public IP addresses, tenant IDs, subscription IDs and resource IDs. Generic labels such as `192.168.x.x` are used where an example is necessary.

## Time handling

Sentinel's display timezone was corrected to Nigeria local time (UTC+01:00). The underlying `TimeGenerated` field remains UTC. When reproducing a timeline, use the raw UTC timestamp for query boundaries and understand that the portal may render it in local time.
