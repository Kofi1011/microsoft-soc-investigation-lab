# Microsoft SOC Investigation Lab

A hands-on cybersecurity project using **Microsoft Sentinel**, **Microsoft Entra ID**, and **Kusto Query Language (KQL)** to investigate authentication activity and build custom detections.

## Featured project: Password-spray investigation

[Read the investigation →](investigations/01-password-spray/README.md)

Analyzed failed sign-ins across multiple accounts, built a scheduled detection, investigated a generated incident, and refined the rule's scheduling and alert context.

| Investigation result | Finding |
| --- | --- |
| Failed sign-ins from the investigated source | **8** |
| Distinct users targeted | **7** |
| Successful authentications from that source | **0** |
| Detection | Custom scheduled Sentinel analytics rule |
| Incident disposition | Authorized security testing |
| Rule tuning | 10-minute schedule and lookback, IP entity mapping, and related-alert grouping |

The lab demonstrates an investigation workflow from log analysis through detection, incident review, classification, and tuning. The results apply to the investigated source and time scope.

## Skills demonstrated

- **Log analysis:** Aggregated sign-in failures and checked authentication outcomes with KQL.
- **Detection engineering:** Created a rule for failed sign-ins against at least three users from one IP within a fixed 10-minute bin.
- **Incident investigation:** Reviewed a Sentinel incident and distinguished authorized test activity from suspected malicious activity.
- **Detection tuning:** Aligned scheduling with the lookback period and added entity mapping and investigation context.
- **Technical communication:** Documented findings, evidence, decisions, and detection limitations.

## Explore the work

| Artifact | Purpose |
| --- | --- |
| [Investigation walkthrough](investigations/01-password-spray/README.md) | Approach, rule settings, and tuning decisions |
| [KQL queries](investigations/01-password-spray/queries.kql) | Detection and investigation logic |
| [Incident report](investigations/01-password-spray/incident-report.md) | Findings, analysis, and disposition |
| [Validation report](investigations/01-password-spray/validation.md) | Live query checks and six detection test cases |
| [Screenshots](investigations/01-password-spray/screenshots/README.md) | Selected evidence with captions and limitations |

## Project scope

This repository documents **hands-on lab work**, not professional SOC employment or a production deployment. Live validation reproduced the reported authentication totals, and six synthetic detection tests passed. A measured reduction in duplicate incidents has not been established.

Published evidence omits real account names, email addresses, tenant and subscription IDs, and public source IPs. Query examples use a documentation-only IP address.
