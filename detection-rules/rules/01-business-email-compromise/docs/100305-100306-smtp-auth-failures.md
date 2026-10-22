# 100305 / 100306 - SMTP authentication failures

| | |
|---|---|
| **Rule IDs** | 100305 (base, level 3) and 100306 (correlation, level 12) |
| **Category** | 01 - Business Email Compromise |
| **Status** | Drafted - log source not yet confirmed |
| **MITRE ATT&CK** | [T1110 - Brute Force](https://attack.mitre.org/techniques/T1110/) |

## Threat description

Before a BEC attacker can read or send mail as a victim, they need the password. Repeated failed SMTP logins from one address point to guessing, spraying, or stuffing leaked credentials.

## Data sources required

A mail server that writes SMTP authentication failures to a log Wazuh collects. The rules currently assume a Windows event with ID **1035** and an `action` field equal to `failure`. **That is an assumption.** Capture a real failed-login event from your mail server and adjust the fields to match.

## Detection logic (plain English)

100305 matches each SMTP auth failure (low severity, used only as a building block). 100306 fires when the same source IP causes 3 or more of them within 600 seconds.

## Rule code

- Wazuh: [`../wazuh/local_rules.xml`](../wazuh/local_rules.xml)

## Test procedure

Produce 3+ failed SMTP logins from one source within 10 minutes, then search `rule.id:100306`. Record how many failures it took to fire; Wazuh's `frequency` counting should be confirmed by observation. _Not yet run._

## False positive analysis

Misconfigured mail clients with an old password retry constantly. Tune by raising the count, excluding known internal sources, or requiring a different user name per attempt for a spraying rule.

## Known limitations

`<same_source_ip />` needs the source IP to be decoded for this log type; otherwise correlation never groups events.

## References

- MITRE ATT&CK T1110: <https://attack.mitre.org/techniques/T1110/>
- Threat intelligence sources: _to add_

## Change log

- 2026-10-21 - Technique changed from T1110.001 to T1110 (the description mentions both guessing and stuffing). Removed the unrelated duplicate DNS rule 100304 (see CHANGELOG).
