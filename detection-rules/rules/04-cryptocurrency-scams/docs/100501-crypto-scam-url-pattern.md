# 100501 - Crypto-scam URL pattern

| | |
|---|---|
| **Rule ID** | 100501 (Wazuh) |
| **Category** | 04 - Cryptocurrency scam patterns |
| **Severity** | Medium-high (level 10) |
| **Status** | Drafted |
| **MITRE ATT&CK** | [T1204.001 - User Execution: Malicious Link](https://attack.mitre.org/techniques/T1204/001/) |

## Threat description

Crypto scams reach Nigerian users through fake airdrops, "double your bitcoin" offers and wallet-drainer pages that trick the victim into signing away their funds. The lure is usually a link.

## Data sources required

Web proxy or web server logs in this format (see `decoders/local_decoder.xml`):

```
fintech-web src_ip=10.0.0.8 method=GET http_uri=/claim-airdrop
```

## Detection logic (plain English)

Alert when a requested URL contains `claim-airdrop`, `double-your-bitcoin` or `free-matic`, in any letter case.

## Rule code

- Wazuh: [`../wazuh/local_rules.xml`](../wazuh/local_rules.xml)
- Decoder: [`../../../decoders/local_decoder.xml`](../../../decoders/local_decoder.xml)

## Test procedure

In `wazuh-logtest`, paste the sample lines in [`tests/sample-logs.md`](../../../tests/sample-logs.md). **Negative test:** a request to `/home` or `/airdrop-faq` must stay silent. _Not yet run._

**Test result:** _not yet recorded._

## False positive analysis

News articles, security blogs and awareness training pages that mention these phrases in a URL. Tune by limiting the match to the path of unfamiliar domains, or by checking the destination domain against an allowlist.

## Known limitations

Keyword matching is easy to evade (rename the page) and is weak on its own. It is a starting point; stronger signals are scam-domain feeds and wallet-drainer script signatures in responses, which the playbook lists for this category.

## Change log

- 2026-10-21 - Added the missing `fintech-web-log` decoder. Changed technique from T1557 (adversary-in-the-middle) to T1204.001. Lowered level 12 to 10 and reworded "known scam site" because the rule matches keywords, not a verified list.
