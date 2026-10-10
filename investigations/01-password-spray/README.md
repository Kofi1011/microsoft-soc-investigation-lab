# Investigation 01: Potential Password Spray

**Hands-on lab project · Microsoft Sentinel · Microsoft Entra ID · KQL**

## Objective

Investigate failed sign-ins across multiple Entra accounts, check for successful authentication, and build a Sentinel detection that provides useful incident context.

## Findings

The investigated source generated **8 failed sign-ins against 7 distinct users**, with **0 successful authentications** in the reviewed scope. A custom scheduled analytics rule generated a Sentinel incident. The investigated activity was documented as authorized **Security testing**.

A subsequent live review reproduced the authentication totals and confirmed that the saved rule was enabled. See the [validation report](validation.md) for test results and evidence limits.

## Investigation approach

1. Reviewed authentication records in `SigninLogs`.
2. Aggregated failed attempts and distinct users by source IP.
3. Filtered to the investigated source and counted both failed and successful sign-ins.
4. Created a scheduled analytics rule to detect failures against at least three users from one source.
5. Reviewed the generated incident and alert, then documented the authorized testing context.
6. Refined scheduling, IP entity mapping, custom details, and related-alert grouping.

## Detection logic

The rule filters nonzero sign-in results, groups failures by source IP and fixed 10-minute time bin, and returns groups involving at least three distinct users.

This identifies a **potential password-spray pattern**. Sign-in logs alone do not establish that one password was used across accounts or that the activity was malicious.

[View the queries](queries.kql)

## Rule configuration

| Setting | Configuration |
| --- | --- |
| Name | Possible Password Spray Against Multiple Entra Users |
| Severity / status | Medium / Enabled |
| Frequency / lookback | Every 10 minutes / last 10 minutes |
| Detection condition | At least 3 distinct users with failed sign-ins from one IP in a fixed 10-minute bin |
| Alert threshold | More than 0 query results |
| Event grouping | All query results into one alert per run |
| IP entity mapping | IP → Address → IPAddress |
| Custom details | FailedAttempts and TargetedUsers columns |
| Incident creation | Enabled |
| Related-alert grouping | Match all entities within 1 hour |
| Reopen closed incidents | Disabled |
| Suppression / automated response | Not configured |
| ATT&CK mapping | Credential Access → T1110 Brute Force → T1110.003 Password Spraying |

The saved rule's status, query, schedule, lookback, severity, ATT&CK mapping, incident creation, and grouping were verified in the portal. Entity mapping, custom details, and reopening behavior are documented from the retained rule summary. That summary contains the display-label typo `TargetdUsers`, mapped to the correct `TargetedUsers` column; its final correction has not been independently verified.

## Tuning decisions

The initial screenshot shows a 5-minute schedule with a 1-hour lookback. A later configuration used a 10-minute lookback while still running every 5 minutes. The saved rule now runs every 10 minutes with a 10-minute lookback.

Aligning the interval with the lookback removes the configured overlap between consecutive query windows. IP mapping supplies an investigation entity; custom details expose the failure and user counts; related-alert grouping is intended to consolidate recurring alerts.

These settings address sources of repeated detections, but the lab has no controlled before-and-after measurement of incident reduction. Grouping alerts also does not suppress rule execution.

## Detection limitations

- **Fixed bins:** Three users split across a bin boundary can fail to reach the threshold in either bin. The query does not evaluate every possible rolling 10-minute interval.
- **Broad failure filter:** Nonzero results include different failure reasons. Further triage should inspect the result codes and descriptions.
- **Shared or distributed sources:** A shared IP can combine unrelated users; activity spread across several IPs can remain below the per-source threshold.
- **Scheduling and ingestion:** Equal frequency and lookback do not guarantee complete coverage when logs arrive late.
- **Alert context:** Grouping every result into one alert can combine several qualifying IPs. Matching all entities may then group differently from a rule that creates one alert per source.
- **Outcome scope:** Zero successes applies to the investigated source and time scope, not the entire environment.

This rule is a tested lab baseline. Production use would require broader data, threshold calibration, and validation of ingestion and grouping behavior.

## Evidence

- [Incident report](incident-report.md)
- [Validation report and repeatable tests](validation.md)
- [Screenshot evidence](screenshots/README.md)
