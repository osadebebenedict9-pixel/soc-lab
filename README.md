# SOC Lab

> My security home lab and the detection-engineering projects I build in it. Each project has its own folder, README and status label.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-work%20in%20progress-yellow.svg)

## What this is

A home SOC environment for practising detection engineering: writing rules, triggering them with simulated activity, and recording what works and what does not. I publish projects here as they reach a documented, reproducible state.

## Lab

- Wazuh 4.14.7 all-in-one (manager, indexer, dashboard) on Ubuntu, in VirtualBox
- Windows 11 agent
- Kali Linux agent

Detailed lab documentation (network layout, agent setup, log sources) is still to be added.

## Projects

| Project | What it is | Status |
|---|---|---|
| [detection-rules](detection-rules/) | Wazuh detection rules for threats targeting Nigerian fintech (BEC, SIM swap / account takeover, banking-trojan C2, crypto scams), mapped to MITRE ATT&CK, with one documentation page per rule | v0.1, rules **Drafted**, not yet **Tested** |

More projects will be added as they are finished.

## Status labels

| Label | Meaning |
|---|---|
| Drafted | Written and reviewed; not yet confirmed in the lab |
| Tested | Evidence of the rule firing, and staying silent on normal activity, is committed |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Report security concerns as described in [SECURITY.md](SECURITY.md).

## Author

Osadebe, self-directed cybersecurity learner focused on detection engineering. GitHub: [@osadebebenedict9-pixel](https://github.com/osadebebenedict9-pixel)

## License

[MIT](LICENSE)
