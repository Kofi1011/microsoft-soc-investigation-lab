# Screenshot Evidence

These four original screenshots were reviewed for exposed email addresses, account names, tenant and subscription IDs, and public IP addresses. They are included without edits.

## 1. Initial rule settings

The initial configuration ran every 5 minutes with a 1-hour lookback. This screenshot provides the starting point for the tuning discussion; it is not the final saved configuration.

![Initial scheduled rule settings](initial-rule-settings.png)

## 2. Generated Sentinel incident

A Medium Sentinel incident is visible with one alert. The screenshot supports incident generation, but does not show a saved classification.

![Generated Medium Sentinel incident](sentinel-incident.png)

## 3. Repeated incidents before tuning

The incident list shows multiple Medium incidents during the lab. This supports the decision to review scheduling and grouping. It does not measure any improvement after tuning.

![Multiple incidents visible during the lab](repeated-incidents.png)

## 4. Classification options

The menu includes Security testing under informational, expected activity. This shows the available classification path; the open menu does not prove that the choice was saved.

![Security testing classification option](classification-options.png)

## Privacy and evidence limits

The original authentication-total screenshot was reviewed privately and omitted because it exposes the public source IP and user principal names. Its counts—8 failures, 7 users, and 0 successes—were reproduced during live validation.

Portal screenshots containing account information or subscription IDs were also omitted. The [validation report](../validation.md) records checks of the saved rule and explains which details were independently verified.
