# Microsoft SOC Investigation Lab

Hands-on lab/project work using Microsoft Sentinel, Microsoft Entra sign-in logs, and Kusto Query Language (KQL). This portfolio demonstrates investigation and detection engineering practice; it does not represent professional SOC employment or production incident response.

## Featured investigation

[01 — Password spray](investigations/01-password-spray/README.md)

Investigated a potential password spray, validated authentication outcomes, generated a Sentinel incident through a custom scheduled rule, classified the activity as **Security testing**, and tuned detection scheduling and incident grouping.

| Observed outcome | Result |
| --- | --- |
| Failed sign-ins | 8 |
| Targeted users | 7 |
| Successful authentications | 0 |
| Detection | Custom Sentinel scheduled analytics rule |
| Incident | Generated and investigated |
| Classification | Security testing |
| Tuning | Schedule aligned with lookback; IP entity mapping and related-alert grouping |

## Skills demonstrated

- KQL aggregation and authentication-outcome analysis
- Microsoft Entra sign-in log investigation
- Sentinel scheduled analytics rules and entity mapping
- Incident triage, evidence interpretation, and classification
- Detection tuning and clear investigation documentation

## Repository guide

- [Investigation walkthrough](investigations/01-password-spray/README.md)
- [KQL queries](investigations/01-password-spray/queries.kql)
- [Incident report](investigations/01-password-spray/incident-report.md)
- [Screenshot evidence and limitations](investigations/01-password-spray/screenshots/README.md)

## Evidence and privacy

Results are documented from the lab conversation and reviewed screenshots. Only screenshots without exposed account identifiers, tenant/subscription IDs, or public IP addresses are included. Query examples use a documentation-only IP placeholder. This is a small authorized lab dataset, not a benchmark of production detection effectiveness.
