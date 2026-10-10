# Incident Report: Potential Password Spray

**Environment:** Authorized Microsoft Sentinel and Entra ID lab  
**Configured severity:** Medium  
**Documented disposition:** Security testing

## Executive summary

A custom Sentinel scheduled analytics rule generated an incident for failed sign-ins across multiple accounts. Investigation of the selected source identified **8 failed sign-ins against 7 distinct users and 0 successful authentications**.

The activity was documented as authorized security testing. The finding demonstrates detection and investigation of suspicious authentication behavior in a lab; it does not establish a malicious compromise.

## Scope and evidence

The investigation focused on one source IP and its Entra sign-in records. The original results screenshot used a Last hour query scope. A subsequent source-scoped check over the preceding 24 hours reproduced the same totals.

| Finding | Supporting evidence |
| --- | --- |
| 8 failures, 7 users, 0 successes | Original aggregation screenshot reviewed privately; totals reproduced during live validation |
| Incident generation | Included screenshot shows a Medium Sentinel incident with one alert |
| Repeated incidents | Included screenshot shows multiple incidents before tuning |
| Authorized testing disposition | Lab record identifies Security testing; live incident records show one closed benign incident with the reason SuspiciousButExpected |
| Saved detection | Portal review confirms the rule is enabled with a 10-minute frequency and lookback |

The classification menu screenshot shows an available option, not a saved choice. The live incident record supports a broader benign/expected disposition; it does not independently establish the exact Security testing subcategory.

## Analysis

Failures across several users from one source warranted investigation for possible password spraying. Counting successful outcomes for the same source helped determine whether successful authentication was visible in the reviewed records.

No successful authentication was observed in this scope. That finding supports a conclusion of unsuccessful attempts for this source; it is not an environment-wide compromise assessment.

The authorized lab context supported the Security testing disposition. No containment or remediation action is documented.

## Detection tuning

The rule's frequency was aligned with its 10-minute lookback. IP entity mapping and count custom details were configured to improve investigation context. Related-alert grouping was configured for one hour, with reopening of closed incidents disabled in the retained summary.

The tuning has not been shown to reduce incident volume through a controlled comparison. During live validation, the latest matching incident records included **one closed benign incident and 14 incidents still marked New**. The documented disposition applies to the investigated activity, not every generated incident.

## Follow-up

- Confirm the final custom-detail display label is spelled `TargetedUsers`.
- Review and disposition the remaining lab incidents.
- Inspect failure codes and distinguish invalid credentials from other authentication failures.
- Validate late-arriving events, time-bin boundaries, and grouping across multiple source IPs.
- Measure incident volume before and after tuning with a consistent test dataset.

## Outcome

The lab demonstrated KQL analysis, custom detection, incident generation and review, classification, and detection tuning. Authentication totals were reproduced during validation, and six synthetic tests confirmed the documented detection behavior.

[Validation details](validation.md) · [Queries](queries.kql) · [Screenshots](screenshots/README.md)
