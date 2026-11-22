---
type: reference
title: OKF Folder Criteria
description: Public, versioned criteria for a folder of working documents used by AI tools.
timestamp: 2026-11-21
owner: JidoSeal
status: draft
review_policy: Reviewed at each version and at least every 12 months
reviewed_at: 2026-11-21
next_review_at: 2027-11-21
---

# OKF Folder Criteria

Public, versioned criteria for the documents your AI tools work from. Current version: **[1.0.0](CRITERIA.md)**. It has four grades: OKF Certified, Bronze, Silver and Gold. Version 1.0.0 renames the grades, so read the name map in the [change log](CHANGELOG.md) before you compare with [v0.9.2](https://github.com/Jidoseal/okf-folder-criteria/blob/v0.9.2/CRITERIA.md).

OKF Certified checks the three OKF v0.2 section 11 rules. For conformity, each document must also state its type, title, description, date and accountable owner (Bronze), and the folder needs a fingerprint tied to a source revision. Silver adds review fields. Gold adds AI-provenance fields.

Pick a folder, scan it, and compare the result with these rules. Or apply the rules by hand and keep a technical file.

**This is not full OKF validation.** OKF Certified covers the three OKF v0.2 section 11 rules only. See [section 7](CRITERIA.md#7-the-okf-check).

On the release date of 1.0.0, no JidoSeal tool applies it yet: the scanners apply v0.9.2. See [section 3](CRITERIA.md#3-how-to-run-the-check).

| File | What it is |
|---|---|
| [CRITERIA.md](CRITERIA.md) | The criteria and check rules |
| [CHANGELOG.md](CHANGELOG.md) | What changed, by version |
| [schema/](schema/) | JSON Schema for parsed frontmatter and for the evidence record |
| [data/key-aliases.json](data/key-aliases.json) | Native-language field spellings the scanner accepts |
| [data/spdx-licenses.json](data/spdx-licenses.json) | SPDX License List 3.29.0 ids, used by Gold check V5 |
| [examples/](examples/) | Small folders with expected results (`expected-1.0.json` for v1.0.0, worked out by hand) |

**Report an error:** [open an issue](https://github.com/Jidoseal/okf-folder-criteria/issues) or write to support@jidoseal.com. See [section 11](CRITERIA.md#11-report-an-error-or-comment).

**Scheme owner:** JidoSeal, which also sells certificates against these criteria. See [section 10](CRITERIA.md#10-how-jidoseal-uses-these-criteria).

These criteria are not a Google document or an ISO standard. JidoSeal holds no ISO accreditation. Nobody endorses them, and JidoSeal is not affiliated with Google or ISO.

**Licence:** [CC BY 4.0](LICENSE). The licence covers this repository only. It does not give any right to the JidoSeal name or marks.

The scanner's source is not part of this repository. This repository holds the rules, the schema, the data and the examples.

The `examples` folders contain files that fail on purpose. Both scan paths skip them here, through `.jidoseal/config.yaml`.
