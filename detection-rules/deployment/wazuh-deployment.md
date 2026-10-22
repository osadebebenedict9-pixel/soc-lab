# Deploying on Wazuh

Written against Wazuh 4.14.7 (all-in-one install on Ubuntu).

## Files

| Repository file | Install location on the manager |
|---|---|
| `decoders/local_decoder.xml` | `/var/ossec/etc/decoders/local_decoder.xml` |
| each category's `wazuh/local_rules.xml` | merged into `/var/ossec/etc/rules/local_rules.xml` |

Back up first: `sudo cp -r /var/ossec/etc/rules /var/ossec/etc/decoders ~/wazuh-backup/`

## Steps

1. Copy the decoder file and merge the rule groups you want. Keep rule IDs unique (the custom range is 100000-120000).
2. Test the configuration: `sudo /var/ossec/bin/wazuh-analysisd -t`
3. Restart: `sudo systemctl restart wazuh-manager`
4. Test with `sudo /var/ossec/bin/wazuh-logtest` using [`tests/sample-logs.md`](../tests/sample-logs.md).

## Prerequisites for rule 100302

- Sysmon installed on the Windows agent with DNS query logging (Event ID 22) enabled in its configuration.
- The agent collects the Sysmon channel. In the agent's `ossec.conf`:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

- The `<if_sid>` in the rule matches the Sysmon DNS rule on your manager: `grep -rn "Event 22" /var/ossec/ruleset/rules/`

## References

- Custom rules: <https://documentation.wazuh.com/current/user-manual/ruleset/rules/custom.html>
- Custom decoders: <https://documentation.wazuh.com/current/user-manual/ruleset/decoders/custom.html>
- Testing decoders and rules: <https://documentation.wazuh.com/current/user-manual/ruleset/testing.html>
