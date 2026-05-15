# Reviewer Brief

Use the template INSIDE the fenced block below when spawning `reviewer_heavy` during `Wave 3` for higher-risk completed cleanup work:

```text
LANE ASSIGNMENTS:
[Lane n: lane name, or MERGE: Lane n + Lane m]

PARENT-SYNTHESIZED LANE CONSTRAINTS:
- [constraints carried forward from Wave 1 and triage]

ACCEPTED WAVE 1 PROOF:
- [specific evidence and lane-specific proof that justified the implemented cleanup]

INTENDED CLEANUP EDITS:
- [what the worker was supposed to change]

RISK NOTES:
- [known correctness or regression risks]

ALLOWED WRITE SCOPE:
- [files or directories the worker was allowed to edit]

VALIDATION EXPECTED:
- [commands expected before final response]

GOAL:
Review the completed cleanup work for regressions, correctness risks, over-broad edits, missed validation, violations of the parent-synthesized lane constraints, changes outside allowed write scope, and changes that exceed the accepted Wave 1 proof.

NEVER spawn subagents unless explicitly asked to!
Stay read-only. Do not implement fixes, run fixers, format files, update snapshots, install dependencies, or modify repo files.

CHANGED FILES:
- [files changed by the worker, each on new line]

VALIDATION:
- [validation already run and any failures or skipped checks]

AVOID:
- [unrelated files, lanes, or work outside this review scope]

Include the following info in final response:
- Findings first, ordered by severity.
- Whether the work stayed inside the validated brief, accepted Wave 1 proof, and allowed write scope.
- Missing, weak, failed, or stale validation.
- Whether follow-up should return to `Wave 2`.
- Residual risk if no issues are found.
```
