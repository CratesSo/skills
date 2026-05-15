# Wave 2 Worker Brief

You must use the template in the fenced block below when spawning worker agents for `Wave 2`.

```text
VALIDATED IMPLEMENTATION BRIEF:
- Lane assignment: [Lane n: lane name, or MERGE: Lane n + Lane m]
- Exact files in scope: [files tied to implementation scope, each on new line]
- Exact cleanup edits: [specific edits approved by parent triage]
- Lane constraints carried forward: [constraints parent synthesized from Wave 1 evidence]
- Accepted Wave 1 proof: [specific evidence and lane-specific proof that justify these edits]
- Risk notes: [known risk and boundaries]
- Allowed write scope: [files or directories this worker may edit]
- Disallowed files/work: [overlapping files, lanes, generated/vendor areas, or other exclusions]
- Validation expected: [commands parent expects after implementation]

GOAL:
Implement only the validated cleanup brief with the smallest correct edit.
NEVER spawn subagents.
Use `Accepted Wave 1 proof` as the boundary for what may change; don't expand into new lanes or unrelated files.
If the brief is internally inconsistent, lacks enough proof to implement safely, or requires files outside `Allowed write scope`, stop and report the blocker instead of improvising.
Don't install dependencies, modify manifests/lockfiles, regenerate snapshots, or run broad formatters unless validated brief explicitly requires it.

RELEVANT FILES:
- [files tied to scope, each on new line]

AVOID:
- [any overlapping files/work owned by parallel worker agents, generated/vendor areas, and anything outside the validated brief]

Use exact schema below for final report:

## SUMMARY
- Summarize changes, including whether edits stayed within the accepted Wave 1 proof, or exact blocker/deviation and why.

### Files Changed
- list files

### Validation Run
- list with pass/fail/blocked status

### Validation Skipped
- list why, plus what parent should run

### Residual Risks
- list only if material, otherwise omit section
```
