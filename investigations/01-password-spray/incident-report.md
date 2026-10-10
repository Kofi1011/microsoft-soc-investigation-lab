# Incident Report: Potential Password Spray

## Scope

Hands-on Microsoft Sentinel and Entra lab investigation of authorized security testing. Real source IPs, account names, tenant IDs, and subscription IDs are intentionally omitted.

## Summary

A custom scheduled Sentinel analytics rule detected failed Entra sign-ins across multiple accounts and generated an incident. Investigation of the selected source showed **8 failures, 7 targeted users, and 0 successful authentications** in the reviewed scope. The activity was classified as **Security testing**.

## Evidence and analysis

| Finding | Evidence |
| --- | --- |
| Authentication totals | Reviewed screenshot of the source-scoped KQL aggregation: 8 failed, 0 successful, 7 distinct users |
| Incident generation | Included incident screenshot shows a Medium Sentinel incident and one alert |
| Repeated incident creation | Included incident-list screenshot shows multiple Medium incidents before tuning |
| Classification | Lab conversation records Security testing; included menu screenshot shows the available classification, not proof of a saved classification |
| Final configuration | Latest pasted rule summary records 10-minute frequency/lookback, IP mapping, counts, and 1-hour grouping |

The authentication-total screenshot is omitted because it exposes the public source IP and user principal names. Counts are reproduced in text without those identifiers.

## Investigation decisions

Failures across multiple users warranted investigation for possible spraying. Counting successful outcomes for the same source distinguished observed attempts from observed successful authentication. The zero-success result limited the finding to unsuccessful activity in this scope.

Authorized lab context supported **Security testing** classification rather than treating the exercise as a confirmed malicious compromise. No containment or remediation action is claimed.

## Tuning and follow-up

The schedule was aligned to the 10-minute lookback. IP entity mapping and count custom details provide investigation context; 1-hour entity-based grouping associates related alerts. Reopening closed incidents was disabled.

A final custom-detail spelling correction was requested in the conversation but is not confirmed by the retained evidence. Retesting after tuning, measuring duplicate counts, reviewing failure codes, and validating boundary/ingestion behavior remain useful follow-up work; none is claimed complete.

## Outcome

The lab demonstrated authentication-log analysis, custom detection, incident generation and investigation, classification, and detection tuning. No successful authentications were observed in the investigated scope.
