# 100301 - Email forwarding attribute changed in Active Directory

| | |
|---|---|
| **Rule ID** | 100301 (Wazuh) |
| **Category** | 01 - Business Email Compromise |
| **Severity** | High (level 12) |
| **Status** | Drafted - not yet tested |
| **MITRE ATT&CK** | [T1114.003 - Email Collection: Email Forwarding Rule](https://attack.mitre.org/techniques/T1114/003/) |

## Threat description

After taking over a mailbox, BEC attackers often set up forwarding so a copy of every message goes to an address they control. This lets them watch invoices and payment conversations and time their fraud. The change can be quiet: the victim's mailbox looks normal.

## Data sources required

- Windows Domain Controller with **Directory Service Changes auditing** enabled (Security Event **5136**)
- Wazuh agent on the Domain Controller collecting the Security channel

## Detection logic (plain English)

Alert when a directory object is modified (event 5136) and the attribute that changed is `forwardingAddress`.

## Rule code

- Wazuh: [`../wazuh/local_rules.xml`](../wazuh/local_rules.xml)

## Before you rely on it

- Confirm which attribute your environment changes when forwarding is set. `forwardingAddress` is an assumption. Mail systems differ, and forwarding set in Exchange Online or Microsoft 365 does not produce a Domain Controller event at all. Check a real event before calling this rule tested.
- Field name case: the rule uses `win.eventdata.attributeLDAPDisplayName` (first letter lowercased by Wazuh). Verify in an alert's JSON.

## Test procedure

Needs a lab Domain Controller. On a test user, set a forwarding address with Active Directory Users and Computers or PowerShell, then search `rule.id:100301`. _Not yet run._

## Expected alert output

`rule.id` 100301, level 12, MITRE T1114.003, description `BEC: Email forwarding attribute modified in Active Directory...`

**Test result:** _not yet recorded._

## False positive analysis

Helpdesk staff and mail-migration projects legitimately set forwarding. Tune by excluding known admin accounts (`win.eventdata.subjectUserName`) and migration windows.

## References

- MITRE ATT&CK T1114.003: <https://attack.mitre.org/techniques/T1114/003/>
- Threat intelligence sources: _to add_

## Change log

- 2026-10-21 - Lowercased field name, reworded description.
