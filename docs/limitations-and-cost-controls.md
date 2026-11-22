# Limitations, interpretation and cost controls

## Evidence limitations

- The outbound connection test to `example.com` was benign and operator-initiated. Sysmon records a connection; it does not by itself establish malicious intent.
- The three remote SMB failures were intentionally caused by entering an incorrect password. They demonstrate failed network logon telemetry, not an external brute-force attack.
- Windows recorded the SMB failure source address as a private address different from the supporting server's configured interface address. This can occur with virtual networking/NAT. The event's recorded source field is reported as observed rather than replaced with an assumed address.
- Scheduled-task creation and execution were observed, but the available process-correlation evidence did not conclusively attribute the task creator to a specific process. Attribution is therefore not claimed.
- The KQL queries are designed for this lab's rendered event format. Different Windows builds, event rendering, locale or collection paths may require field-extraction adjustments.

## Time handling

The Sentinel portal display timezone was corrected to Nigeria local time (UTC+01:00). `TimeGenerated` is stored in UTC. Query windows in the KQL files use UTC where explicit timestamps are needed. A one-hour difference between UTC and local display is not evidence of missing events.

## Cost controls

- Keep the DCR scoped to the event IDs required for the investigations.
- Review Log Analytics usage and Azure Cost Management regularly.
- Stop or deallocate lab VMs when not in use.
- Remove temporary test artefacts after collecting evidence.
- Do not enable additional data sources, retention or paid capabilities without reviewing the cost implications first.
- No exact cost figure is asserted in this repository.

## Security and privacy

Screenshots in this repository were cropped and redacted to remove account and directory details, security identifiers (SIDs), and source and destination IP addresses. Anyone reproducing the lab and sharing screenshots should likewise redact:
- Azure tenant and subscription identifiers.
- Resource IDs and account details.
- Public IP addresses and personally identifying information.
- Password prompts, tokens, secrets or credentials.

## Scope exclusions

The project does not demonstrate a real compromise, malware execution, data exfiltration, command-and-control, automated incident response or full enterprise detection coverage.
