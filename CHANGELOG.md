---
type: changelog
title: Change log
description: What changed in the OKF Folder Criteria, by version.
timestamp: 2026-11-21
owner: JidoSeal
status: draft
review_policy: Reviewed at each version and at least every 12 months
reviewed_at: 2026-11-21
next_review_at: 2027-11-21
---

# Change log

## 1.0.0, 2026-11-21

A new grade ladder. The grade names Bronze, Silver and Gold now mean different bars. Read the name map before you compare a v1.0.0 grade with a v0.9.2 grade.

**Name map.** It covers the field rules only:

| v0.9.2 | v1.0.0 |
|---|---|
| (none) | OKF Certified: O1 to O3, new |
| Bronze: F1 | OKF Certified, if every file also passes O1 and O2 (and the folder passes O3). Otherwise no v1.0.0 grade. |
| Silver: F1 to F5 | Bronze |
| Gold: F1 to F5 and G1 to G4 | Silver |
| (none) | Gold: Silver plus V1 to V6, new |

In one line: v0.9.2 Gold = v1.0 Silver.

- **New lowest grade, OKF Certified.** Rules O1, O2 and O3 are the three OKF v0.2 section 11 rules. Every grade above it needs them. In v0.9.2 they were a separate line and no grade depended on them.
- **The map is not exact for every file.** A file that reached v0.9.2 Silver only because rule P1 repaired it (leading tabs, full-width colons), or because its `type` key is spelled in another language, passes F1 to F5 but fails O1 or O2. It is not v1.0.0 Bronze. A folder whose `index.md` or `log.md` fails O3 is graded `none`.
- **New grade, Gold: AI provenance.** Checks V1 to V6 test that `ai_involvement`, `sources`, `retrieved_at`, `generated`, `verified` and `license` are present, well-formed and agree with each other. They do not test that the values are true, and make no network calls. Drafts of this version called them P1 to P5; they are V1 to V6 here because P1 to P3 already name the parsing rules. V6 is the draft's consistency rule on `generated.by`.
- **Conformity** now needs Bronze on every in-scope file, O3 on the folder, and R1 to R3. That is the v0.9.2 bar (F1 to F5) plus the OKF section 11 rules.
- **Coverage and score** use four grades. The score is the mean of four coverage figures, so it cannot be compared with a v0.9.2 score.
- Rule P1 now says which rules use the repairs. O1 uses none.
- Section 9 states what Bronze, Silver and Gold do not show. Section 10 states two certificate rules: at least 25 in-scope files, and a certificate keeps the criteria version and grade name it was issued under.
- New files: `schema/okf-folder-criteria-1.0.schema.json`, `schema/evidence-1.0.schema.json` (accepts `criteria_version` 1.0.0), `data/spdx-licenses.json` (SPDX License List 3.29.0, the 708 current ids, used by V5) and `examples/expected-1.0.json`. The 0.9 schema files and `examples/expected.json` stay as they were.
- `examples/expected-1.0.json` was worked out by hand from this document. No tool computed it.
- **No tool applies v1.0.0 on its release date.** The browser Self-Check and the command line up to version 0.1.5 apply v0.9.2. This log will record when each tool applies v1.0.0.
- Certificates issued under v0.9.2 do not change. They keep their v0.9.2 grade name.

## 0.9.2, 2026-11-02

A wording change. Field rules, grades and conformity do not change.

- Section 11 no longer has an appeals route. The checks are binary and repeatable, so there is a report-an-error route instead: re-run the published check, email support@jidoseal.com with the result, and a confirmed tool error is fixed and re-run at no charge.
- The sample assessor clause asks for published criteria and a route to report an error.
- The evidence schema accepts `criteria_version` 0.9.2.

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
