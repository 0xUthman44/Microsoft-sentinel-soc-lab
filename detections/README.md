# KQL detections and investigations

These queries were developed for the lab's Log Analytics `Event` table and the rendered event descriptions observed during testing.

| File | Purpose | Evidence |
|---|---|---|
| [`powershell-discovery.kql`](powershell-discovery.kql) | PowerShell 4104 discovery activity | [Scenario 01](../docs/scenarios/01-powershell-execution-and-discovery.md) |
| [`powershell-discovery-categorised.kql`](powershell-discovery-categorised.kql) | Same events with a discovery category label (fixed UTC window) | [Scenario 01](../docs/scenarios/01-powershell-execution-and-discovery.md) |
| [`registry-runkey-persistence.kql`](registry-runkey-persistence.kql) | Sysmon 13 Run/RunOnce value modifications | [Scenario 02](../docs/scenarios/02-registry-runkey-persistence.md) |
| [`scheduled-task-correlation.kql`](scheduled-task-correlation.kql) | Timeline of 4104, 4688 and 4698 (fixed UTC window) | [Scenario 03](../docs/scenarios/03-scheduled-task-persistence.md) |
| [`powershell-outbound-network.kql`](powershell-outbound-network.kql) | Sysmon 3 PowerShell network activity | [Scenario 04](../docs/scenarios/04-outbound-network-investigation.md) |
| [`remote-smb-failed-logons.kql`](remote-smb-failed-logons.kql) | Security 4625 failures from the controlled SMB test (fixed UTC window) | [Scenario 05](../docs/scenarios/05-authentication-investigation.md) |
| [`authentication-investigation-window.kql`](authentication-investigation-window.kql) | Same 4624/4625 timeline in a fixed UTC window | [Scenario 05](../docs/scenarios/05-authentication-investigation.md) |
| [`authentication-investigation.kql`](authentication-investigation.kql) | Security 4624/4625 authentication timeline | [Scenario 05](../docs/scenarios/05-authentication-investigation.md) |

## Usage

Run each query in the `law-sentinel-soc-lab` workspace. The queries use a 24-hour lookback unless a specific time window is defined. Adjust the time range for later reproductions.

The extraction expressions depend on the event's `RenderedDescription` format. If the provider, Windows version or collection path changes, inspect a raw event and adjust the extraction patterns before relying on the output.

The discovery query is a reference query for the controlled commands listed in Scenario 01 (`.exe` command forms). The Scenario 01 screenshot shows a categorised view of the results, which `powershell-discovery-categorised.kql` produces.
