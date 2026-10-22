# Tests

Every rule gets three checks before it is marked **Tested**:

1. **It fires** when the malicious behaviour is simulated.
2. **It stays silent** on normal activity (negative test).
3. **Edge cases** behave sensibly (different letter case, near-miss values).

Sample inputs are in [`sample-logs.md`](sample-logs.md). Put evidence (screenshots, `wazuh-logtest` output) in `tests/evidence/`, named after the rule ID, for example `100302-fired.png` and `100302-silent.png`.

Status labels used across the repository:

| Label | Meaning |
|---|---|
| Drafted | Written and reviewed; not yet confirmed in the lab |
| Needs decoder / data source | Cannot fire until a log source or decoder exists |
| Tested | Evidence of firing and of silence on normal activity is in `tests/evidence/` |
