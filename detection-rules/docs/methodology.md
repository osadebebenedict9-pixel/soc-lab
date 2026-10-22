# Methodology

Each rule follows the same six steps.

1. **Threat research.** Find at least two authoritative sources and one real incident write-up. Identify the tactics, techniques and procedures, map them to MITRE ATT&CK, and note what the threat looks like in logs or network traffic.
2. **Rule design.** Choose the data source, the condition that matches the threat but not legitimate activity, the acceptable false-positive rate, the noise sources and how to filter them, and the severity.
3. **Implementation.** Write the rule in more than one format (Wazuh XML, Sigma, Snort) to show the detection is not tied to one tool.
4. **Testing.** Simulate the behaviour in the lab and confirm the rule fires; run without the behaviour and confirm silence; test edge cases; write the procedure so a reviewer can repeat it.
5. **False-positive analysis.** List the legitimate activity that could trigger the rule and how to tune for each.
6. **Documentation.** One page per rule: threat, MITRE mapping, data sources, logic, rule code, test procedure, expected output, false positives, tuning, references, change log.

## Honesty rules for this repository

- A rule is **Drafted** until evidence of it firing and staying silent is committed.
- Indicators must name their source. Placeholders are labelled as placeholders.
- Descriptions say only what the rule can actually detect (for example, a lookalike pattern is not proof a domain is newly registered).
- Corrections are recorded in the changelog.
