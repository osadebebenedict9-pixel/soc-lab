# 100201 / 100202 - New device registrations (SIM swap / account takeover)

| | |
|---|---|
| **Rule IDs** | 100201 (base, level 3) and 100202 (correlation, level 12) |
| **Category** | 02 - SIM swap and account takeover |
| **Status** | Drafted - test with `wazuh-logtest` |
| **MITRE ATT&CK** | [T1078 - Valid Accounts](https://attack.mitre.org/techniques/T1078/) |

## Threat description

SIM swap fraud lets an attacker receive a victim's SMS one-time passwords, then log in to the victim's banking or wallet app from a new device. In the application's own logs, the visible sign is a device registration, often several in a short time as the attacker works through accounts or retries.

## Data sources required

Application authentication logs in this format (see `decoders/local_decoder.xml`):

```
fintech-auth event_type=device_registered result=success user_id=u1001 device_id=dev-A geo_location=Lagos,NG
```

## Detection logic (plain English)

100201 records every successful device registration. 100202 fires when the same `user_id` registers devices again within 15 minutes.

## Rule code

- Wazuh rules: [`../wazuh/local_rules.xml`](../wazuh/local_rules.xml)
- Decoder: [`../../../decoders/local_decoder.xml`](../../../decoders/local_decoder.xml)

## Test procedure

1. Run `sudo /var/ossec/bin/wazuh-logtest`.
2. Paste the sample lines from [`tests/sample-logs.md`](../../../tests/sample-logs.md) in one session. Check that the decoder shows `user_id`, `device_id` and `geo_location` and that 100201 matches.
3. Send further registrations for the same user. Note which line triggers 100202 (it may need 3 lines, not 2).
4. **Negative test:** register devices for two different users; 100202 must stay silent.

## Expected alert output

100201 at level 3 for each registration; 100202 at level 12 with the user ID in the description.

**Test result:** _not yet recorded._

## False positive analysis

Customers who buy a new phone and reinstall the app, or who register a tablet and a phone together. Tune by checking geolocation change between registrations, or by requiring a recent SIM-change or OTP-reset event in the same window.

## Known limitations

The rule only sees registrations. It does not yet correlate with OTP resets, login location, or transaction-limit changes, which the playbook lists for this category.

## Change log

- 2026-10-21 - Decoder now outputs `user_id` (was `user`, which broke `$(user_id)` and grouping). Replaced `<same_id>` with `<same_field>`. Added T1078.
