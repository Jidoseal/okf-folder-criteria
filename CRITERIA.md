---
type: specification
title: OKF Folder Criteria v0.9
description: The public check rules for a folder of working documents used by AI tools.
timestamp: 2026-11-01
owner: JidoSeal
status: draft
review_policy: Reviewed at each version and at least every 12 months
reviewed_at: 2026-11-01
next_review_at: 2027-11-01
---

# OKF Folder Criteria, version 0.9

| | |
|---|---|
| Version | 0.9, draft for comment |
| Date | 2026-11-01 |
| Scheme owner | JidoSeal |
| Written for | Folders in Open Knowledge Format (OKF) v0.2 |
| Change log | [CHANGELOG.md](CHANGELOG.md) |
| Machine-readable form | [schema/okf-folder-criteria-0.9.schema.json](schema/okf-folder-criteria-0.9.schema.json) |

## 1. What this is

A folder of documents can guide an AI tool's work. These criteria say what each document must state, and what evidence must tie the folder to its source.

This document states what the scanner checks today. Where the scanner does not check something, this document says so.

**This is not full OKF validation.** Section 7 lists the gap.

This is not a Google document. It is not an ISO standard. JidoSeal holds no ISO accreditation. Nobody endorses these criteria, and JidoSeal is not affiliated with Google, ISO or anyone else named here.

## 2. Terms

- **Folder**: the set of files named as the subject of the evidence.
- **In-scope file**: a file that scope rules S1 to S4 leave in.
- **Populated**: a value that passes rule P2.
- **Rule ID**: a stable name for one rule. Quote it in a contract, a report or an error report.
- **Checked by tool**: the scanner tests it. **Recorded by supplier**: the supplier states it, and the scanner does not test it.

## 3. How to run the check

Pick a folder, scan it. Two scan paths apply these rules today: the browser Self-Check at jidoseal.com and the JidoSeal command line. Section 8 lists where they differ.

## 4. Scope and parsing rules

| ID | Rule | Who checks |
|---|---|---|
| S1 | A file is in scope if its name ends in `.md`, in lower case. | Tool |
| S2 | `index.md` and `log.md` are out of scope at any depth. OKF reserves these names. | Tool, command line only (section 8) |
| S3 | Files and folders whose names start with a dot are out of scope. | Tool, command line only (section 8) |
| S4 | The supplier may exclude paths by regular expression, under `machine_extra_excludes` in `.jidoseal/config.yaml`. A reader should ask to see this file. | Tool, command line only (section 8) |
| S5 | A symbolic link that leads outside the folder stops the whole scan. | Tool, command line only |
| P1 | Frontmatter is the block between a first line that is exactly `---` and a later line that is exactly `---`. A byte order mark is removed first. Leading tabs are expanded. Full-width colons on recognised keys are repaired. | Tool |
| P2 | A value is populated unless it is null, a blank or whitespace-only string, an empty list or an empty mapping. Numbers, booleans and dates are populated. | Tool |
| P3 | If there is no frontmatter, the block has no closing line, the YAML does not parse, or the block is not a mapping, nothing in the file is populated. | Tool (section 8 lists one difference) |

## 5. Field rules

Each rule passes if at least one listed key is populated.

### Silver fields: required for conformity

| ID | Field | Keys that count |
|---|---|---|
| F1 | Type | `type` |
| F2 | Title | `title` |
| F3 | Description | `description`, `desc` |
| F4 | Date | `timestamp`, `updated`, `created`, `date`, `generated.at` |
| F5 | Accountable owner | `owner`, `author`, `generated.by` |

### Gold fields: optional higher grade

| ID | Field | Keys that count |
|---|---|---|
| G1 | Lifecycle status | `status` |
| G2 | Review policy | `review_policy` |
| G3 | Last review date | `reviewed_at` |
| G4 | Next review date | `next_review_at` |

The scanner also accepts native-language spellings for F1 to F5 and G1. They are listed in [data/key-aliases.json](data/key-aliases.json), for 11 locales. G2, G3 and G4 have none. `stale_after` is recognised, and is not required.

### What the field rules do not check

- **Presence only.** The scanner does not judge any value.
- A `next_review_at` date in the past still passes G4.
- F5 passes on any populated value. The owner need not be a real person.
- F4 passes on any populated value. It need not be a valid date.
- The scanner does not read the body of a document.

## 6. Grades and conformity

### File grade

| Grade | A file needs |
|---|---|
| none | F1 fails |
| Bronze | F1 |
| Silver | F1 to F5 |
| Gold | F1 to F5 and G1 to G4 |

### Folder result

- **Folder grade**: the lowest grade of any in-scope file. A folder with no in-scope files is `none`.
- **Coverage**: for each of Bronze, Silver and Gold, the percentage of in-scope files at that grade or above, to one decimal place.
- **Score**: the mean of the three coverage figures, rounded half up to a whole number.

There is no minimum file count.

### Evidence rules

| ID | Rule | Who checks |
|---|---|---|
| R1 | **Fingerprint.** Record the root hash of the folder's in-scope files, computed as below. | Tool |
| R2 | **Revision tie.** Record the source revision the scan ran on: a git commit or a dated snapshot. | Recorded by supplier |
| R3 | **Which folder.** Record the folder path that the evidence covers. Also record the criteria version and the date. | Recorded by supplier |

The fingerprint does not read git. The scanner does not test R2 or R3. They are a statement by the supplier, in the form of [schema/evidence-0.9.schema.json](schema/evidence-0.9.schema.json).

**R1 method.**

1. For each in-scope file, take the SHA-256 of its raw bytes. The leaf is `sha256:` plus the first 16 hex characters.
2. Order the leaves by sorted relative path.
3. To join a pair, take the SHA-256 hex of the two strings joined left then right, as UTF-8. If a level has an odd count, repeat its last entry.
4. Repeat until one value is left. That is the root.
5. A folder with no in-scope files has the SHA-256 of empty input as its root.

A one-byte change in a file changes the root. So does adding or removing an in-scope file.

### Conformity

A folder conforms to OKF Folder Criteria v0.9 if:

1. every in-scope file is Silver or Gold, and
2. the evidence records R1, R2 and R3.

Bronze is not enough. It tests only the OKF `type` field. Gold is an optional higher grade, and is not needed for conformity.

A supplier may show conformity without any JidoSeal tool. Apply the rules in this document and keep a technical file: one entry per rule ID, the result, and how it was found. The [examples](examples/expected.json) show the expected result for two small folders.

## 7. The OKF gap: what is not checked today

OKF v0.2 section 11 sets three conformance rules. Bronze does not cover all three. **A grade here does not mean the folder conforms to OKF.** We do not validate full OKF today.

| OKF section 11 rule | What the scanner does today | Status |
|---|---|---|
| 1. Every non-reserved `.md` file has a parseable YAML frontmatter block. | No check of its own. A file with no readable block gets grade `none`. The parser repairs some errors (rule P1), so a file can pass here and fail a strict YAML parser. | Planned for the next version |
| 2. Every frontmatter block has a non-empty `type`. | Tested by F1. A native-language key also counts, so a file can pass F1 with no key spelled `type`. | Tested, with that exception |
| 3. `index.md` and `log.md` follow their own sections when present. | Not tested. The command line skips these files. The browser Self-Check grades them as ordinary files, and does not test their structure. | Planned for the next version |

The next version plans a separate result line: "OKF section 11: x of 3 rules pass". The plan leaves Bronze, Silver and Gold unchanged.

The other way round, F2 to F5 and G1 to G4 go beyond OKF. OKF requires only `type`. Missing these fields does not make a file fail OKF.

## 8. Where the two scan paths differ

Results can differ between the paths. Compare like with like, and name the path in the evidence.

| Case | Command line | Browser Self-Check |
|---|---|---|
| `index.md`, `log.md` | Skipped | Graded |
| Dot-files and dot-folders | Skipped | Graded, except the folders listed below |
| `.jidoseal/config.yaml` excludes | Applied | Not read |
| Folders `node_modules`, `.git`, `.github`, `.next`, `dist`, `build`, `extensions`, `venv`, `.venv`, `virtualenv`, `site-packages`, `__pycache__`, `.openclaw`, and `docs/plugins`, `docs/providers`, `docs/reference` | Graded, unless a dot-folder or excluded | Skipped |
| Some malformed YAML, such as `type: [unclosed` | Fails to parse, grade `none` | Reads the value as text, can reach Bronze or above |
| Fingerprint of a file with a byte order mark | Hashes raw bytes | Hashes the decoded text, so the root can differ |

Because of these differences, the browser path can leave out a file the command line grades, and the other way round. These are differences in the tools as they stand today. They are not extra rules. See [examples/expected.json](examples/expected.json) for both results on one folder.

## 9. What these criteria do not show

A pass does not show that the content is correct or that any instruction is safe. It does not show that the supplier is secure or complies with any law. It does not show that an AI tool reads the folder.

## 10. How JidoSeal uses these criteria

- JidoSeal writes these criteria, grades against them and sells certificates. Its tools can also fill in missing fields. That is a conflict of interest. Weigh it.
- A buyer's clause may require an assessor that did not fix or advise on the folder. Check that before you rely on any certificate, including ours.
- JidoSeal issues a certificate from the grade, score and root that the customer's own scan reports. JidoSeal does not see the files and does not compute the result again. A person reviews after issue.
- These criteria do not name a sole assessor. Anyone may assess against them.

## 11. Report an error, or comment

This is a draft. We want comments, above all from buyers and from people who build checkers.

- Open an issue: <https://github.com/Jidoseal/okf-folder-criteria/issues>
- Or write to support@jidoseal.com

To report an error, give the version, the rule ID, a small example file, what you expected and what you saw. If the scanner and this document disagree, tell us which path you used. We will correct one of them and record it in the change log.

An issue here is not an appeal against a certificate decision. This repository does not provide an appeals route.

## 12. Versions

Each version is a tagged release that does not change once published. A correction gets a new version and a change log line. Cite the version number in a contract.
