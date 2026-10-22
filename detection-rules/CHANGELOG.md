# Changelog

## v0.1.0 - 2026-10-21

First public version: 8 Wazuh rules, decoders for 3 log formats, per-rule documentation.

Corrections made during review of the original drafts:

- **100302:** corrected field name to `win.eventdata.queryName` (Wazuh lowercases the first letter), removed a redundant duplicate field line, made matching case-insensitive, and reworded the description because the rule cannot tell whether a domain is newly registered.
- **100301:** lowercased `attributeLDAPDisplayName`.
- **100304 removed:** it duplicated 100302 and depended on a `dns_query` field that no decoder produces, so it could never fire.
- **100306:** MITRE mapping T1110.001 changed to T1110.
- **Auth decoder:** field renamed from `user` to `user_id` so `$(user_id)` in descriptions and grouping in 100202 work.
- **100202:** `<same_id>user_id</same_id>` replaced with `<same_field>user_id</same_field>`.
- **100401:** dots in the IP escaped; "known Banking Trojan C2" wording removed because the IP is a lab placeholder.
- **100501:** added the missing `fintech-web-log` decoder; MITRE T1557 changed to T1204.001; level 12 lowered to 10; description reworded.

Not yet included: brute-force rules 100700 and 100701 (waiting for the rule text), Sigma and Snort versions.
