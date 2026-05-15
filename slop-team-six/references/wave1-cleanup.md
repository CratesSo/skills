# Wave 1 Cleanup Brief

You must use the template INSIDE the fenced block below when spawning `cleanup` agents for `Wave 1`.

```text
LANE ASSIGNMENT:
[Lane n: lane name, or MERGE: Lane n + Lane m]

LANE REFERENCE:
- [absolute path to each assigned lane reference]
Read the listed lane reference(s) before analysis and apply them as Wave 1 discovery guidance.

TOOL REFERENCES:
- [absolute path to each relevant tool reference, or N/A]
Read listed tool reference(s) before running matching tools.

AVAILABLE TOOLS AND COMMANDS:
- Tool/check: [real runnable command from parent preflight or lane-installed tool]
  Use for: [what signal it proves for this lane]
  Inspect: [specific output, files, warning class, or graph]
- Tool/check: [real runnable command from parent preflight or lane-installed tool]
  Use for: [what signal it proves for this lane]
  Inspect: [specific output, files, warning class, or graph]

GOAL:
[describe their task/lane with actionable info, success condition, and relevant lane discovery constraints]

NEVER spawn subagents.
Stay read-only against the target repo: never implement changes.
Don't run fixers, formatters, snapshot updates, repo dependency installs, or code generators that modify the target repo.
ONLY report findings.

RELEVANT FILES:
- [files tied to lane scope, each on new line]

AVOID:
- [any overlapping files owned by parallel agents]

REPORT:
Write `N/A` only when a field truly does not apply.
Don't replace required fields with prose.
Use the assigned lane reference(s)' `Wave 1 Report Checklist` to fill lane-specific evidence fields.
If no findings, still fill `## Rollup` and `## No-Finding Areas`.

Use this schema exactly for your final report:

## ROLLUP
### Lane reference read
- absolute lane reference path(s)
### Tool references read
- absolute tool reference path(s), or N/A
### Commands unavailable or failed:
- command and reason, or N/A
### Scope covered
- files, dirs, symbols, or package areas inspected
### Exclusions
- areas avoided because they were out of scope, owned by another agent, generated, vendor, or risky
### Lane checklist coverage
- assigned lane checklist items considered, and any not applicable

## FINDINGS
### L[n]-F1: [short name]
- **Target**:
  - exact file, directory, symbol, type, dependency, or pattern
- **Evidence**:
  - concrete evidence with paths and enough detail for parent triage
- **Lane-specific proof**:
  - concise proof matching the assigned lane reference(s)' report checklist
- **Why it matters**:
  - why this is material cleanup signal, not style preference
- **Risk/uncertainty**:
  - what may make this noisy, unsafe, dynamic, public, or ambiguous
- **Parent decision needed**:
  - what parent must verify before accepting or implementing, or N/A
- **Confidence**: [high | medium | low]

(Repeat as `L[n]-F2`, `L[n]-F3`, etc. for additional findings)

(Don't include proposed edits or parent action hints. Include concise command/search provenance only when needed to make evidence reproducible. Include validation expectations only when needed for parent triage. Don't omit lane-specific proof just because it overlaps with `Evidence` or `Risk/uncertainty`)

## No-Finding Areas
- **[area checked]**: why no material finding was found

## Notes
- **Uncertainty**: facts parent should verify before Wave 2
- **Out-of-scope escalations**: signals that belong outside this lane assignment
- **Blockers**: missing context, inaccessible files, unavailable commands, write-only commands that would modify the repo, or parent-approved tools that could not be installed or run
```
