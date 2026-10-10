# Validation Report

**Review date:** October 9, 2026 (America/Toronto)  
**Scope:** Published documentation, KQL execution, saved rule settings, incident records, and screenshot privacy.

## Live query checks

The following checks ran successfully in the lab's Azure Monitor Logs interface:

| Check | Execution scope | Observed result |
| --- | --- | --- |
| Detection logic | SigninLogs, preceding 24 hours; source IPs and account arrays removed only from the displayed output | 3 qualifying source/time-bin rows, including one with 8 failures and 7 users |
| Authentication totals | Investigated source, preceding 24 hours | 8 failures, 0 successes, 7 distinct users |
| Detailed triage fields and sorting | Investigated source, preceding 24 hours; results aggregated for privacy | 8 rows, all failures, 0 successes |
| Latest incident records | Matching rule title, preceding 24 hours; latest record per incident number | 1 closed BenignPositive / SuspiciousButExpected incident; 14 New incidents |

The 24-hour validation scope differs from the original screenshot's Last hour scope. Matching totals corroborate the finding, but do not recreate the original query execution exactly.

No new sign-in attempts were generated. These checks queried existing lab data; they do not demonstrate a new scheduled alert after tuning.

## Saved rule review

The Azure portal showed the custom rule as **Enabled**, **Scheduled**, and **Medium**, with **T1110.003 Password Spraying** under Credential Access. Its query matched the repository's detection logic.

The rule panel also confirmed a 10-minute frequency, 10-minute lookback, more-than-zero result threshold, one alert per run, incident creation, and one-hour grouping that matches all entities.

IP mapping, custom-detail mappings, and the disabled reopening setting are retained from the earlier summary. Their saved state was not independently rechecked. The custom-detail display-label correction remains unverified.

## Synthetic detection tests

[Run detection-tests.kql](detection-tests.kql) separately in Logs. It uses synthetic data and documentation-only IP addresses. It does not ingest events or create incidents.

All six cases returned `Passed = true` when executed in Azure Monitor Logs.

| Case | Expected qualifying rows | Actual qualifying rows | Interpretation |
| --- | --- | --- | --- |
| Three users fail from one IP in one bin | 1 | 1 | Threshold is met |
| Only two users fail | 0 | 0 | Below the threshold |
| One user fails repeatedly | 0 | 0 | Repeated attempts do not increase distinct-user count |
| Two failures plus one successful user | 0 | 0 | Successful sign-ins are excluded |
| Three users split across two IPs | 0 | 0 | Sources are evaluated independently |
| Three users split across a bin boundary | 0 | 0 | Confirms a fixed-bin coverage limitation |

The boundary test passes because the observed result matches the documented implementation. It exposes a detection gap; it is not evidence that the rule detects every spray within any rolling 10-minute interval.

The fixture adds a case-name grouping field to isolate scenarios. The failure filter, aggregation functions, source/time grouping, and threshold remain the same as the saved detection.

## Technical review

- The detection preserves `TimeGenerated` through the time-bin grouping.
- Numeric result-code comparisons executed successfully. Microsoft also uses them in its SigninLogs examples, although the table schema lists ResultType as a string.
- `dcount` estimates distinct values at large cardinalities; its default small-set exact-count behavior is appropriate for this seven-user lab dataset.
- Matching frequency and lookback removes configured window overlap. It does not prove duplicate elimination or immunity to delayed ingestion.
- One alert per run can contain multiple qualifying sources. Entity grouping therefore requires further testing with multi-source results.

## Publication review

All five original text files were read in full and revised for grammar, structure, consistent terminology, and evidence scope. The selected screenshots were reviewed visually. Real source IPs and account identifiers are not included in the published text or images.

The report separates observed lab results, live validation, retained settings, and unverified follow-up work. It makes no claim of professional employment, production readiness, or measured duplicate reduction.

## References

- [Microsoft Sentinel scheduled analytics rules](https://learn.microsoft.com/en-us/azure/sentinel/scheduled-rules-overview) — scheduling, thresholds, grouping, and ingestion delay
- [Create scheduled analytics rules](https://learn.microsoft.com/en-us/azure/sentinel/tutorial-detect-threats-custom) — query validation and TimeGenerated requirement
- [SigninLogs query examples](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/queries/signinlogs) — success/failure aggregation
- [SigninLogs table schema](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/signinlogs) — fields and result codes
- [KQL dcount function](https://learn.microsoft.com/en-us/kusto/query/dcount-aggregation-function) — distinct-count behavior
