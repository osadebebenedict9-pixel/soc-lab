# 100302 - Flutterwave lookalike domain DNS query

| | |
|---|---|
| **Rule ID** | 100302 (Wazuh) |
| **Category** | 01 - Business Email Compromise |
| **Severity** | High (level 12) |
| **Status** | Drafted - not yet confirmed firing in the lab |
| **MITRE ATT&CK** | [T1583.001 - Acquire Infrastructure: Domains](https://attack.mitre.org/techniques/T1583/001/) |

## Threat description

Attackers register domains that look like a payment provider's brand, such as `flutterwave-secure.xyz` or `flutter-wave-pay.net`. They use them in phishing and BEC emails to collect credentials or redirect payments. A DNS lookup for such a name from an internal machine is an early sign that someone opened a lure.

This rule watches endpoint DNS queries for names that begin with `flutterwave` (also `flutter-wave` and `flutter_wave`) and are not on the official `.com`, `.ng` or `.co.za` domains.

## Data sources required

- Windows endpoint with **Sysmon** logging **Event ID 22 (DNSEvent)**
- Wazuh agent collecting the `Microsoft-Windows-Sysmon/Operational` event channel
- Wazuh manager default Sysmon rules loaded (the rule uses one as its parent)

## Detection logic (plain English)

When a Sysmon DNS query event arrives, check whether the queried name starts with "flutterwave" in any of its spellings and ends in a domain other than the three official ones. If so, raise a level 12 alert showing the domain and the process that made the query.

## Rule code

- Wazuh: [`../wazuh/local_rules.xml`](../wazuh/local_rules.xml)
- Sigma: _in progress_
- Snort: _in progress_

## Before you rely on it - two things to verify

1. **Parent rule.** The rule uses `<if_sid>61635</if_sid>`. Confirm that is the Sysmon Event 22 rule on your manager: `grep -rn "Event 22" /var/ossec/ruleset/rules/`
2. **Field name.** Wazuh lowercases the first letter of Windows event field names, so the field is `win.eventdata.queryName`, not `QueryName`. Check an alert's JSON to confirm.

## Test procedure

1. On the Windows agent, open PowerShell and run `nslookup flutterwave-secure.xyz` (the domain does not need to exist; the query is what gets logged).
2. In the Wazuh dashboard, open Threat Hunting > Events (last 15 minutes) and search `rule.id:100302`.
3. **Negative tests** (the rule must stay silent): `nslookup flutterwave.com`, `nslookup flutterwave.ng`, `nslookup google.com`.
4. **Edge cases:** `nslookup FLUTTERWAVE-pay.xyz` (case), `nslookup flutter-wave.net`, `nslookup secure-flutterwave.com` (expected to be missed, see limitations).

## Expected alert output

A Wazuh alert with `rule.id` 100302, `rule.level` 12, MITRE T1583.001, and a description such as: `BEC: DNS query to Flutterwave lookalike domain: flutterwave-secure.xyz by C:\Windows\System32\nslookup.exe`.

**Test result:** _not yet recorded - add a screenshot to `tests/evidence/` and change the status to Tested._

## False positive analysis

| Source | Why it triggers | Tuning |
|---|---|---|
| Flutterwave's own staging or regional domains | Not on the three allowed endings | Add legitimate endings to the lookahead list |
| Security teams and researchers | They query lookalikes on purpose | Exclude by agent or by process name |
| Brand-protection or domain-monitoring vendors | They resolve many lookalikes | Allowlist the vendor's resolver or host |

## Tuning recommendations

- Match the brand anywhere in the name, not only at the start (to catch `secure-flutterwave.com`), then test the false positive rate again.
- Add the other brands from the playbook (banks, Paystack, Moniepoint) as separate rules or one CDB list.
- Add a domain-age check by enriching with a threat-intel feed. The rule does not know whether a domain is new.

## Known limitations

- Misses names where the brand is not at the start of the query.
- Does not catch lookalikes that use look-alike characters (homoglyphs).
- Needs Sysmon Event 22. Without it, no alert can fire.

## References

- Wazuh custom rules: <https://documentation.wazuh.com/current/user-manual/ruleset/rules/custom.html>
- Wazuh rules syntax: <https://documentation.wazuh.com/current/user-manual/ruleset/ruleset-xml-syntax/rules.html>
- MITRE ATT&CK T1583.001: <https://attack.mitre.org/techniques/T1583/001/>
- Threat intelligence sources (playbook step 1 requires at least two): _to add_

## Change log

- 2026-10-21 - Removed duplicate field line, made match case-insensitive, corrected field name case, reworded description (the rule cannot tell whether a domain is newly registered).
