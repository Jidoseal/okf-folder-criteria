---
type: specification
title: OKF Folder Criteria v1.0.0
description: The public check rules for a folder of working documents used by AI tools.
timestamp: 2026-11-21
owner: JidoSeal
status: published
review_policy: Reviewed at each version and at least every 12 months
reviewed_at: 2026-11-21
next_review_at: 2027-11-21
---

# OKF Folder Criteria, version 1.0.0

| | |
|---|---|
| Version | 1.0.0 |
| Date | 2026-11-21 |
| Scheme owner | JidoSeal |
| Written for | Folders in Open Knowledge Format (OKF) v0.2 |
| Change log | [CHANGELOG.md](CHANGELOG.md). Version 1.0.0 renames the grades: read the name map there before you compare a v1.0.0 grade with a v0.9.2 grade. |
| Machine-readable form | [schema/okf-folder-criteria-1.0.schema.json](schema/okf-folder-criteria-1.0.schema.json) (a partial aid; this document is the rule) |
| Previous version | [v0.9.2](https://github.com/Jidoseal/okf-folder-criteria/blob/v0.9.2/CRITERIA.md), unchanged at its tag |

## 1. What this is

A folder of documents can guide an AI tool's work. These criteria say what each document must state, and what evidence must tie the folder to its source.

There are four grades, each built on the one below: OKF Certified, Bronze, Silver and Gold.

**This is not full OKF validation.** OKF Certified covers the three conformance rules in OKF v0.2 section 11 only. Section 7 says how each is read.

This is not a Google document. It is not an ISO standard. JidoSeal holds no ISO accreditation. Nobody endorses these criteria, and JidoSeal is not affiliated with Google, ISO or anyone else named here.

## 2. Terms

- **Folder**: the set of files named as the subject of the evidence.
- **In-scope file**: a file that scope rules S1 to S4 leave in.
- **Populated**: a value that passes rule P2.
- **Scan date**: the UTC date the scan starts. The evidence records it (rule R3).
- **ISO date**: a value that matches `^\d{4}-\d{2}-\d{2}(T[0-9:.]+(Z|[+-]\d{2}:\d{2}))?$` and is a real calendar date. An unquoted YAML date, which a YAML parser reads as a date, also counts as an ISO date.
- **Rule ID**: a stable name for one rule. Quote it in a contract, a report or an error report.
- **Checked by tool**: the scanner tests it. **Recorded by supplier**: the supplier states it, and the scanner does not test it.

## 3. How to run the check

Pick a folder, scan it, and compare the result with these rules.

**On the release date of v1.0.0, no JidoSeal tool applies these rules yet.** The browser Self-Check at jidoseal.com and the JidoSeal command line up to version 0.1.5 apply [v0.9.2](https://github.com/Jidoseal/okf-folder-criteria/blob/v0.9.2/CRITERIA.md). Command line version 0.2.0 is planned to apply v1.0.0, and to keep the v0.9.2 rules behind `--criteria 0.9.2`. The change log will record when each tool applies v1.0.0. Until then, apply these rules by hand (section 6), or cite v0.9.2.

## 4. Scope and parsing rules

| ID | Rule | Who checks |
|---|---|---|
| S1 | A file is in scope if its name ends in `.md`, in lower case. | Tool |
| S2 | `index.md` and `log.md` are out of scope at any depth. OKF reserves these names. Rule O3 checks them. | Tool |
| S3 | Files and folders whose names start with a dot are out of scope. | Tool |
| S4 | The supplier may exclude paths by regular expression, under `machine_extra_excludes` in `.jidoseal/config.yaml`. A reader should ask to see this file. If the file has an unknown key, a wrong type or a pattern that will not compile, the scan is refused. | Tool (section 8 lists the differences) |
| S5 | A symbolic link that leads outside the folder stops the whole scan. | Tool, command line only |
| P1 | Frontmatter is the block between a first line that is exactly `---` and a later line that is exactly `---`. A byte order mark is removed first. For the field rules (F, G and V), leading tabs are expanded and full-width colons on recognised keys are repaired. Rule O1 makes no repairs. | Tool |
| P2 | A value is populated unless it is null, a blank or whitespace-only string, an empty list or an empty mapping. Numbers, booleans and dates are populated. | Tool |
| P3 | If there is no frontmatter, the block has no closing line, the YAML does not parse, or the block is not a mapping, nothing in the file is populated. | Tool (section 8 lists one difference) |

## 5. Rules

### OKF Certified rules: the three OKF v0.2 section 11 rules

| ID | Rule | Applies to |
|---|---|---|
| O1 | The file has a frontmatter block that parses as a YAML mapping when read strictly: a byte order mark is removed, and nothing else is repaired. | Each in-scope file |
| O2 | The frontmatter has a populated key spelled exactly `type`. A native-language spelling does not count here. | Each in-scope file |
| O3 | Every `index.md` and `log.md` in the folder follows its OKF section. Section 7 says how. A folder with neither file passes. | The folder |

### Bronze fields

Each rule passes if at least one listed key is populated.

| ID | Field | Keys that count |
|---|---|---|
| F1 | Type | `type` |
| F2 | Title | `title` |
| F3 | Description | `description`, `desc` |
| F4 | Date | `timestamp`, `updated`, `created`, `date`, `generated.at` |
| F5 | Accountable owner | `owner`, `author`, `generated.by` |

### Silver fields

| ID | Field | Keys that count |
|---|---|---|
| G1 | Lifecycle status | `status` |
| G2 | Review policy | `review_policy` |
| G3 | Last review date | `reviewed_at` |
| G4 | Next review date | `next_review_at` |

The scanner also accepts native-language spellings for F1 to F5 and G1. They are listed in [data/key-aliases.json](data/key-aliases.json), for 11 locales. G2, G3 and G4 have none. A file that spells `type` in another language passes F1 and fails O2, so it is not Bronze. `stale_after` is recognised, and is not required.

### Gold checks: AI provenance

Each check fails if a field it needs is missing. No check makes a network call: a URL is checked for form only, and never fetched.

| ID | Field | Check |
|---|---|---|
| V1 | `ai_involvement` | Exactly one of `none`, `assisted`, `generated`. |
| V2 | `sources` | Present and a list. Each entry has `author`, a non-empty string, and `resource`, which is one of: a URL matching `^https?://[^\s]+$`; a path, relative to the folder root, to a file that exists in the folder; or a scheme id matching `^(doi\|isbn\|urn\|arxiv\|issn):[^\s]+$`. Anything else fails. An empty list passes only when `ai_involvement` is `none`. |
| V3 | `sources[].retrieved_at` | Required on each entry whose `resource` is a URL. An ISO date, not after the scan date. |
| V4 | `generated`, `verified` | When `ai_involvement` is `assisted` or `generated`: `generated.by` and `generated.at` are present, `generated.at` is an ISO date, and `verified` is a list with at least one entry whose `by` matches `^human:[A-Za-z0-9._@-]+$` and whose `at` is an ISO date on or after `generated.at` and not after the scan date. |
| V5 | `license` | One of `proprietary`, `internal`, `LicenseRef-` followed by letters, digits, dots or hyphens, or a current id in the SPDX License List version 3.29.0, as copied in [data/spdx-licenses.json](data/spdx-licenses.json). Expressions with `AND`, `OR` or `WITH` are not accepted. |
| V6 | `generated.by` | When present, matches `^(human\|process\|team):[A-Za-z0-9._@-]+$`. |

When an ISO date has a time, V3 and V4 compare only its date part, in UTC. SPDX ids match case-sensitively. A V2 path may not start with `/` or contain a `..` segment.

`sources`, `generated` and `verified` are OKF v0.2 fields. `ai_involvement`, `license` and `retrieved_at` inside a `sources` entry are JidoSeal fields. V1 to V6 have no native-language spellings.

### What these rules do not check

- **Form only.** The scanner does not judge whether any value is true.
- A `next_review_at` date in the past still passes G4.
- F5 passes on any populated value. The owner need not be a real person.
- F4 passes on any populated value. It need not be a valid date.
- V2 does not check that a source exists, says what the file says, or was written by its `author`. V4 does not check that a person reviewed the file.
- The scanner does not read the body of a document.

## 6. Grades and conformity

### File grade

Each grade needs everything the grade below it needs.

| Grade | A file needs |
|---|---|
| none | O1 or O2 fails |
| OKF Certified | O1 and O2 |
| Bronze | O1, O2 and F1 to F5 |
| Silver | O1, O2, F1 to F5 and G1 to G4 |
| Gold | O1, O2, F1 to F5, G1 to G4 and V1 to V6 |

In machine-readable output the grades are written `none`, `okf-certified`, `bronze`, `silver` and `gold`.

### Folder result

- **Folder grade**: the lowest grade of any in-scope file. If O3 fails, the folder grade is `none`. A folder with no in-scope files is `none`.
- **Coverage**: for each of OKF Certified, Bronze, Silver and Gold, the percentage of in-scope files at that grade or above, to one decimal place.
- **Score**: the mean of the four coverage figures, taken before rounding, then rounded half up to a whole number. A v1.0.0 score cannot be compared with a v0.9.2 score, which used three figures.

A grade needs no minimum file count. Section 10 states the file count JidoSeal needs before it issues a certificate.

### Evidence rules

| ID | Rule | Who checks |
|---|---|---|
| R1 | **Fingerprint.** Record the root hash of the folder's in-scope files, computed as below. | Tool |
| R2 | **Revision tie.** Record the source revision the scan ran on: a git commit or a dated snapshot. | Recorded by supplier |
| R3 | **Which folder.** Record the folder path that the evidence covers. Also record the criteria version and the scan date. | Recorded by supplier |

The fingerprint does not read git. The scanner does not test R2 or R3. They are a statement by the supplier, in the form of [schema/evidence-1.0.schema.json](schema/evidence-1.0.schema.json).

**R1 method.**

1. For each in-scope file, take the SHA-256 of its raw bytes. The leaf is `sha256:` plus the first 16 hex characters.
2. Order the leaves by sorted relative path.
3. To join a pair, take the SHA-256 hex of the two strings joined left then right, as UTF-8. If a level has an odd count, repeat its last entry.
4. Repeat until one value is left. That is the root.
5. A folder with no in-scope files has the SHA-256 of empty input as its root.

A one-byte change in a file changes the root. So does adding or removing an in-scope file.

### Conformity

A folder conforms to OKF Folder Criteria v1.0.0 if:

1. every in-scope file is Bronze or above,
2. the folder passes O3, and
3. the evidence records R1, R2 and R3.

OKF Certified is not enough for conformity. Silver and Gold are optional higher grades.

A supplier may show conformity without any JidoSeal tool. Apply the rules in this document and keep a technical file: one entry per rule ID, the result, and how it was found. [examples/expected-1.0.json](examples/expected-1.0.json) shows the expected v1.0.0 result for two small folders. Those results were worked out by hand, not by a tool.

## 7. The OKF check

OKF v0.2 section 11 sets three conformance rules. Rules O1, O2 and O3 are those rules, as read here. A folder graded OKF Certified or above passes all three, as read here: on in-scope files, and on the `index.md` and `log.md` files that S3 and S4 leave in. The scanner also reports them on a separate line: "OKF §11: x of 3 rules pass".

| OKF section 11 rule | How it is read here |
|---|---|
| 1. Every non-reserved `.md` file has a parseable YAML frontmatter block. | O1. Read strictly, with no repairs other than removing a byte order mark. So a file can pass P1 and fail O1. A file with no block fails. |
| 2. Every frontmatter block has a non-empty `type`. | O2. The key must be spelled `type`. A native-language key passes F1 but not O2. A file with no block fails. |
| 3. `index.md` and `log.md` follow their own sections when present. | O3. `index.md`: no frontmatter, except that one at the folder root may carry `okf_version`, and at least one heading. `log.md`: any frontmatter must parse, and `##` headings must be dates in `YYYY-MM-DD` form, newest first. A folder with neither file passes. |

The "OKF §11" line appears today on the browser Self-Check results screen and in the command line from version 0.1.5. A certificate bought from the browser stores this result in JidoSeal's record of the certificate. The public certificate page does not show it.

F2 to F5, G1 to G4 and V1 to V6 go beyond OKF. OKF requires only `type`. Missing these fields does not make a file fail OKF.

## 8. Where the two scan paths differ

These are differences in the tools as they stand on the release date, which apply v0.9.2. They are not extra rules. Compare like with like, and name the path in the evidence.

The two paths agree on `index.md` and `log.md`, on dot-files and dot-folders, on `.jidoseal/config.yaml` excludes, and on malformed YAML. Malformed YAML such as `type: [unclosed` fails on both paths and gets grade `none`.

| Case | Command line | Browser Self-Check |
|---|---|---|
| Folders `node_modules`, `.git`, `.github`, `.next`, `dist`, `build`, `extensions`, `venv`, `.venv`, `virtualenv`, `site-packages`, `__pycache__`, `.openclaw`, and `docs/plugins`, `docs/providers`, `docs/reference` | Graded, unless a dot-folder or excluded | Skipped |
| Fingerprint of a file with a byte order mark | Hashes raw bytes | Hashes the decoded text, so the root can differ |
| Frontmatter written with an explicit key (`? type`) or as one flow mapping (`{type: note}`) | Parses | Refused, grade `none` |
| Patterns in `machine_extra_excludes` | Python regular expressions | JavaScript regular expressions. Python-only syntax such as `\A`, `\Z` and `(?P` is refused. `\w`, `\d` and `\b` read only ASCII, so a file name with other characters can match differently |
| Record of `.jidoseal/config.yaml` | sha256 written to the manifest | sha256 shown on the results screen, and stored in JidoSeal's record of a certificate bought from the browser. The public certificate page does not show it |

See [examples/expected-1.0.json](examples/expected-1.0.json) for both results on one folder.

## 9. What these criteria do not show

A pass does not show that the content is correct or that any instruction is safe. It does not show that the supplier is secure or complies with any law. It does not show that an AI tool reads the folder.

Bronze checks fields that help evidence identification of documented information, of the kind ISO 9001 clause 7.5 describes. Silver adds review and change fields. Neither shows conformity with ISO 9001 or with ISO 30401.

Gold checks that provenance and AI-involvement fields exist, are well-formed and agree with each other. It does not check that the fields are true. It is not ISO/IEC 42001 certification, an AI Act conformity assessment, or legal advice.

## 10. How JidoSeal uses these criteria

- JidoSeal writes these criteria, grades against them and sells certificates. Its tools can also fill in missing fields. That is a conflict of interest. Weigh it.
- A buyer's clause may require an assessor that did not fix or advise on the folder. Check that before you rely on any certificate, including ours.
- JidoSeal issues a certificate from the grade, score and root that the customer's own scan reports. JidoSeal does not see the files and does not compute the result again. A person reviews after issue.
- JidoSeal issues a certificate, free or paid, only for a folder with at least 25 in-scope files. This is a rule for JidoSeal certificates. It is not part of any grade.
- A certificate names the criteria version it was issued under, and keeps that version's grade name. A certificate issued as Gold under v0.9.2 stays a v0.9.2 Gold certificate. It is not a v1.0.0 Gold certificate.
- These criteria do not name a sole assessor. Anyone may assess against them.

## 11. Report an error, or comment

We want comments, above all from buyers and from people who build checkers.

- Open an issue: <https://github.com/Jidoseal/okf-folder-criteria/issues>
- Or write to support@jidoseal.com

To report an error, give the version, the rule ID, a small example file, what you expected and what you saw. If the scanner and this document disagree, tell us which path you used. We will correct one of them and record it in the change log.

There is no appeals process, because the checks are binary and repeatable. Anyone can re-run the published check on the same folder and get the same grade. If you think the tool got it wrong, email support@jidoseal.com with the folder's result. A confirmed tool error is fixed, and the folder is re-run at no charge. Details: <https://jidoseal.com/vendors#conflict-and-errors>.

## 12. Versions

Each version is a tagged release that does not change once published. A correction gets a new version and a change log line. Cite the version number in a contract. Earlier versions stay published at their tags.
