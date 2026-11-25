# Lab 11.1 Completion Summary

## Completed work

- Extended [`tools/sanitise.py`](../tools/sanitise.py) with named redaction patterns for Indian mobile numbers (optional `+91`), `HM-######` employee IDs, AWS access key IDs, labeled AWS secret keys, bearer tokens, JWTs, Windows UNC paths, and connection-string passwords (including quoted values).
- Added focused coverage in [`tools/test_sanitise.py`](../tools/test_sanitise.py) for each new pattern.
- Rewrote the five unsafe examples in [`labs/m11/unsafe_prompts.md`](../labs/m11/unsafe_prompts.md), labeling the data, instruction, autonomy, and operational-safety risks and providing safer alternatives.
- Updated the Lab 11.1 steps in [`labs/m11/README.md`](../labs/m11/README.md) to describe the named patterns and tests.

## Verification

- `pytest -q tools/test_sanitise.py` — passed (1 test).
- Ran the sanitiser with `--report` on `data/tickets/raw_tickets_pii.csv`.
- Checked the generated CSV for `Welcome@`, `AKIA`, `Summer2026`, and `eyJ`; the check reported **No secrets found**.
- The report flagged ticket `INC0010247` as `INJECTION_SUSPECT`.

## Generated outputs

- Sanitised tickets: [`outputs/m11/raw_tickets_pii_sanitised.csv`](./m11/raw_tickets_pii_sanitised.csv)
- Redaction report: [`outputs/m11/redaction_report.json`](./m11/redaction_report.json)

## Human review still required

Review the redaction report and sign the redaction checklist before using the sanitized data with AI. The team AI usage policy template and governance checklist were not completed as part of this work.
