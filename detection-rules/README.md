# Nigerian Fintech Detection Rules

_Part of [soc-lab](../README.md)._

> Open-source Wazuh detection rules for threats targeting Nigerian financial services, mapped to MITRE ATT&CK and documented one page per rule.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-v0.1%20work%20in%20progress-yellow.svg)

## What this is

A growing rule pack for four threat categories seen in Nigerian fintech: business email compromise, SIM swap and account takeover, banking-trojan command and control, and crypto scams. Rules are written for **Wazuh**. **Sigma** and **Snort** versions are in progress.

> **Status: v0.1.** 8 Wazuh rules across 4 of 6 planned categories. Every rule is currently labelled **Drafted**: written and reviewed, but not yet confirmed firing in my lab. A rule becomes **Tested** only when its page links evidence of the alert firing and of it staying silent on normal activity.

## Why it matters

Public rule sets cover global threats well but say little about region-specific attacks, such as lookalike domains of Nigerian payment brands or SIM-swap patterns in fintech application logs. This project documents how each rule was designed, tested and tuned so that a SOC analyst can reuse and adapt it.

## Highlights

- One documentation page per rule: threat, data sources, logic, test procedure, false positives, tuning, references, change log
- Custom decoders for three fintech log formats, with sample log lines for `wazuh-logtest`
- Changelog that records every correction, including mistakes found in the first drafts
- Honest status labels and a public list of known gaps

## Rule status

| Category | Rules | Wazuh | Sigma | Snort | Status |
|---|---|:-:|:-:|:-:|---|
| 01 Business email compromise | 100301, 100302, 100305, 100306 | yes | in progress | in progress | Drafted |
| 02 SIM swap / account takeover | 100201, 100202 | yes | - | - | Drafted |
| 03 Banking trojan C2 | 100401 | yes | - | - | Drafted (placeholder indicator) |
| 04 Cryptocurrency scams | 100501 | yes | - | - | Drafted |
| 05 Government impersonation | - | - | - | - | Planned |
| 06 Insider threat | - | - | - | - | Planned |

See [`coverage-matrix.md`](coverage-matrix.md) for the ATT&CK mapping.

## Quick start

1. Copy `decoders/local_decoder.xml` to `/var/ossec/etc/decoders/` on the Wazuh manager.
2. Merge the rules you want into `/var/ossec/etc/rules/local_rules.xml`.
3. Restart: `sudo systemctl restart wazuh-manager`
4. Test with `wazuh-logtest` using [`tests/sample-logs.md`](tests/sample-logs.md).

Full steps and prerequisites: [`deployment/wazuh-deployment.md`](deployment/wazuh-deployment.md).

## Documentation

- [Methodology](docs/methodology.md)
- [Deployment guide](deployment/wazuh-deployment.md)
- [Test procedure and status labels](tests/README.md)
- [Changelog](CHANGELOG.md)

## Reproducing results

Follow [`tests/sample-logs.md`](tests/sample-logs.md) and the "Test procedure" section of each rule page. Save your output in `tests/evidence/`.

## Lab

Wazuh 4.14.7 all-in-one (manager, indexer, dashboard) on Ubuntu in VirtualBox, with a Windows 11 agent and a Kali Linux agent.

## Known gaps

- No rule is marked Tested yet.
- Rule 100401 uses a lab placeholder IP, not a verified indicator.
- The SMTP and Active Directory rules assume log fields that still need to be confirmed against real events.
- Threat-research references for each rule are still to be added.
- Sigma and Snort versions are not published yet.

## Roadmap

1. Record test evidence and promote rules to Tested.
2. Publish Sigma and Snort versions.
3. Add categories 05 and 06 and grow each category to 4-8 rules.
4. Write up the threat landscape and methodology.

## Author

Osadebe, self-directed cybersecurity learner focused on detection engineering. GitHub: [@osadebebenedict9-pixel](https://github.com/osadebebenedict9-pixel)

## Citation

If you reuse these rules, please link back to this repository.

## References

Each rule page lists its own references. Shared documentation:

- [Wazuh custom rules](https://documentation.wazuh.com/current/user-manual/ruleset/rules/custom.html)
- [Wazuh custom decoders](https://documentation.wazuh.com/current/user-manual/ruleset/decoders/custom.html)
- [MITRE ATT&CK](https://attack.mitre.org/)

## License

[MIT](../LICENSE)
