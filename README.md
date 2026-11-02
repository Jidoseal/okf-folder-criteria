---
type: reference
title: OKF Folder Criteria
description: Public, versioned criteria for a folder of working documents used by AI tools.
timestamp: 2026-11-01
owner: JidoSeal
status: draft
review_policy: Reviewed at each version and at least every 12 months
reviewed_at: 2026-11-01
next_review_at: 2027-11-01
---

# OKF Folder Criteria

Public, versioned criteria for the documents your AI tools work from. Current version: **[0.9](CRITERIA.md)**, draft for comment.

Each document must state its type, title, description, date and accountable owner. The folder needs a fingerprint tied to a source revision.

Pick a folder, scan it, and compare the result with these rules. Or apply the rules by hand and keep a technical file.

**This is not full OKF validation.** Today's check covers one of the three OKF section 11 rules as its own test. The rest are planned for the next version. See [section 7](CRITERIA.md#7-the-okf-gap-what-is-not-checked-today).

| File | What it is |
|---|---|
| [CRITERIA.md](CRITERIA.md) | The criteria and check rules |
| [CHANGELOG.md](CHANGELOG.md) | What changed, by version |
| [schema/](schema/) | JSON Schema for parsed frontmatter and for the evidence record |
| [data/key-aliases.json](data/key-aliases.json) | Native-language field spellings the scanner accepts |
| [examples/](examples/) | Small folders with expected results |

**Report an error:** [open an issue](https://github.com/Jidoseal/okf-folder-criteria/issues) or write to support@jidoseal.com. See [section 11](CRITERIA.md#11-report-an-error-or-comment).

**Scheme owner:** JidoSeal, which also sells certificates against these criteria. See [section 10](CRITERIA.md#10-how-jidoseal-uses-these-criteria).

These criteria are not a Google document or an ISO standard. JidoSeal holds no ISO accreditation. Nobody endorses them, and JidoSeal is not affiliated with Google or ISO.

**Licence:** not set yet. We will name one in a later version.

The scanner's source is not part of this repository. This repository holds the rules, the schema, the data and the examples.

The `examples` folders contain files that fail on purpose. The command line skips them here, through `.jidoseal/config.yaml`. The browser Self-Check does not read that file, so scan the repository root on the command line, or expect those failures.
