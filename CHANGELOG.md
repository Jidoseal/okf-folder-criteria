---
type: changelog
title: Change log
description: What changed in the OKF Folder Criteria, by version.
timestamp: 2026-11-01
owner: JidoSeal
status: draft
review_policy: Reviewed at each version and at least every 12 months
reviewed_at: 2026-11-01
next_review_at: 2027-11-01
---

# Change log

## 0.9.1, 2026-11-01

A correction. Field rules, grades and conformity do not change.

- The browser Self-Check now matches the command line on `index.md`, `log.md`, dot-files, `.jidoseal/config.yaml` excludes and malformed YAML. Section 8 and rules S2, S3 and S4 updated.
- Browser results change for folders with those files or that YAML. See `examples/expected.json`.
- Section 7 updated: the scanner now reports "OKF §11: x of 3 rules pass". It is added beside the grade. The command line prints it from version 0.1.5.
- Section 11 points to the appeals route.
- Licence set to CC BY 4.0.
- The evidence schema accepts `criteria_version` 0.9 and 0.9.1.

## 0.9, 2026-11-01

First public draft, for comment.

- Publishes scope and parsing rules, field rules, grades, evidence rules and the conformity test.
- Rules match what the JidoSeal scanner checks on this date, read from its code.
- Conformity needs Silver on every in-scope file, plus R1, R2 and R3.
- Records the OKF section 11 gap. Rule 1 and rule 3 have no check of their own. Planned for the next version.
- Records where the command line and the browser Self-Check differ.
- No licence set yet.
