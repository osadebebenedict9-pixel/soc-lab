# 100401 - Connection to a listed C2 indicator

| | |
|---|---|
| **Rule ID** | 100401 (Wazuh) |
| **Category** | 03 - Banking trojan command and control |
| **Severity** | High (level 12) |
| **Status** | Drafted - indicator is a lab placeholder |
| **MITRE ATT&CK** | [T1071 - Application Layer Protocol](https://attack.mitre.org/techniques/T1071/) |

## Threat description

Banking trojans that target Nigerian users call back to a command-and-control server to receive instructions and send stolen data. A firewall log showing an internal host connecting to a known C2 address is strong evidence of infection.

## Data sources required

Firewall logs in this format (see `decoders/local_decoder.xml`):

```
fintech-firewall src_ip=10.0.0.5 dst_ip=185.220.101.45
```

## Detection logic (plain English)

Alert when any internal host connects to the listed destination address.

## Important: the indicator is a placeholder

`185.220.101.45` is used so the rule can be demonstrated in a lab. It is **not** a verified banking-trojan C2 address. Do not describe it as one. For real use, take indicators from a cited feed (abuse.ch, AlienVault OTX), record the source and date for each, and move them into a CDB list so the rule does not need editing for each indicator.

## Rule code

- Wazuh: [`../wazuh/local_rules.xml`](../wazuh/local_rules.xml)

## Test procedure

In `wazuh-logtest`, paste the sample line above (and the lines in `tests/sample-logs.md`). **Negative test:** change `dst_ip` to any other address; the rule must stay silent.

**Test result:** _not yet recorded._

## False positive analysis

Indicator lists age quickly: addresses get reassigned or belong to shared hosting, so a match may be innocent. Tune by adding indicator expiry dates and checking the source's confidence rating.

## Tuning recommendations

Add the playbook's other C2 detections: DGA-looking domains, unusual DNS TXT queries, malware User-Agent strings, and beaconing at regular intervals.

## Change log

- 2026-10-21 - Escaped the dots in the IP match, removed the "known Banking Trojan" wording, removed the "critical" group tag.
