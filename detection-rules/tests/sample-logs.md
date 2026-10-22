# Sample log lines for `wazuh-logtest`

Run `sudo /var/ossec/bin/wazuh-logtest`, choose the log format `syslog` if asked, and paste one line at a time. Keep a single session open so correlation rules can see earlier events.

## Decoders and rules 100201 / 100202 (account takeover)

```
fintech-auth event_type=device_registered result=success user_id=u1001 device_id=dev-A geo_location=Lagos,NG
fintech-auth event_type=device_registered result=success user_id=u1001 device_id=dev-B geo_location=Abuja,NG
fintech-auth event_type=device_registered result=success user_id=u1001 device_id=dev-C geo_location=Asaba,NG
```

Expected: every line decodes to `event_type`, `result`, `user_id`, `device_id`, `geo_location` and matches 100201. Record which line triggers 100202.

Negative test (should not trigger 100202):

```
fintech-auth event_type=device_registered result=success user_id=u2001 device_id=dev-X geo_location=Lagos,NG
fintech-auth event_type=device_registered result=success user_id=u2002 device_id=dev-Y geo_location=Lagos,NG
```

## Rule 100401 (C2 indicator)

```
fintech-firewall src_ip=10.0.0.5 dst_ip=185.220.101.45
```

Negative test:

```
fintech-firewall src_ip=10.0.0.5 dst_ip=93.184.216.34
```

## Rule 100501 (crypto-scam URL)

```
fintech-web src_ip=10.0.0.8 method=GET http_uri=/claim-airdrop
```

Negative tests:

```
fintech-web src_ip=10.0.0.8 method=GET http_uri=/home
fintech-web src_ip=10.0.0.8 method=GET http_uri=/airdrop-faq
```

## Rule 100302 (needs a real Windows agent)

Not testable with `wazuh-logtest` alone, because it needs a Sysmon Event 22 record. On the Windows agent run `nslookup flutterwave-secure.xyz`, then check the dashboard for `rule.id:100302`. See the rule page for negative tests.

## Recording results

For each test, save a screenshot or the `wazuh-logtest` output in `tests/evidence/` (named after the rule ID) and update the rule page's "Test result" line.
