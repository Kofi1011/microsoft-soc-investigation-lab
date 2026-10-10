# Investigation 01: Potential Password Spray

**Context:** Hands-on Microsoft SOC lab/project work. Authorized security testing, not a production security incident.

## Objective and findings

Identify one source generating failed sign-ins across multiple Entra accounts, determine whether authentication succeeded, and carry the finding through Sentinel detection and investigation.

The investigated source produced **8 failed sign-ins against 7 distinct users and 0 successful authentications** in the reviewed scope. These counts describe the investigation query scope, not necessarily a single scheduled-rule time bin.

## Workflow

1. Review Entra authentication records in `SigninLogs`.
2. Aggregate failures and distinct users by source IP.
3. Scope the investigation to the observed source and count both failed and successful sign-ins.
4. Create a custom Sentinel scheduled analytics rule for failures against at least three distinct users.
5. Review the generated Sentinel incident and its alert.
6. Classify the authorized activity as **Security testing**.
7. Tune scheduling, entity mapping, and alert grouping to reduce repeated incident creation.

## Scheduled analytics rule

| Setting | Documented configuration |
| --- | --- |
| Name | Possible Password Spray Against Multiple Entra Users |
| Description | Detects potential password-spray activity when a single source IP generates failed Microsoft Entra sign-ins against three or more distinct user accounts within a 10-minute window. |
| Severity / status | Medium / Enabled |
| Query | See query 1 in queries.kql |
| Frequency / lookback | Every 10 minutes / last 10 minutes |
| Threshold | More than 0 query results |
| Event grouping | All query results into a single alert |
| Entity mapping | IP → Address → IPAddress |
| Custom details | FailedAttempts and TargetedUsers |
| Incident creation | Enabled |
| Related-alert grouping | Enabled, match all entities, 1 hour |
| Reopen closed incidents | Disabled |
| Suppression / automation | Not configured |
| ATT&CK mapping | Credential Access → T1110 → T1110.003 Password Spraying |

The latest pasted summary still labels the custom detail `TargetdUsers` while mapping it to the correct `TargetedUsers` column. Correcting the display label was recommended; the available evidence does not confirm that final save.

## Detection tuning

Earlier evidence shows a 5-minute frequency with a 1-hour lookback. A later pasted summary shows a 5-minute frequency with a 10-minute lookback. The latest summary aligns both frequency and lookback to 10 minutes, maps the IP entity, and includes counts in custom details.

These changes reduce overlapping query coverage and allow grouping using the source IP. The screenshots show multiple incidents before tuning. No post-tuning measurement is available, so this project does not claim a measured reduction or elimination of duplicates.

## Interpretation and limitations

- Multiple failed accounts from one IP are a useful lead; they do not establish the password used or prove malicious password spraying.
- The broad nonzero-result filter includes different sign-in failure reasons; investigation should examine those reasons.
- Zero successful authentications applies to the reviewed source and time scope. It does not prove that the entire environment was uncompromised.
- Fixed 10-minute bins can split activity at a boundary. Log ingestion delays and distributed source IPs can affect coverage.
- The rule is a lab baseline. Thresholds and grouping require validation before production use.
- KQL examples retain investigation logic; supplementary queries are reusable examples, not claimed historical executions.

See the [incident report](incident-report.md) and [screenshots](screenshots/README.md).
