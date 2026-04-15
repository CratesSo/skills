---
name: audit-team
description: User triggered.
---
# CANONICAL FLOW
1. You lock audit lens, scope, and budgets.
2. You spawn one `navigator`.
3. `navigator` maps repo area relevant to audit lens and defines default exclusions.
4. `navigator` creates non-overlapping `reviewer` slices by path or subsystem.
5. `navigator` spawns `reviewer` wave.
6. Reviewers return confirmed findings.
7. `navigator` triages `reviewer` output, rejects weak findings, merges duplicates, and groups accepted findings into non-overlapping implementation slices.
8. `navigator` chooses `worker_mini` or `worker` per implementation slice.
9. `navigator` spawns the worker wave for accepted implementation slices.
10. Workers return changes made.
11. `navigator` consolidates full run and wraps concise report for you.
12. You run final validation.

# YOUR RESPONSIBILITIES
- Lock audit lens and include/exclude scope.
- Spawn exactly one `navigator`.
- Wait for `navigator` -> `reviewer` + `worker`.
- Run final validation after `navigator` returns.

# NAVIGATOR PROMPT
Spawn `navigator` with `fork_context=false`.
Copy only the contents of the fenced block below into the spawned prompt. Do not include this heading or any wrapper text.

```md
### AUDIT LENSE
- concise description of audit lens

### NAVIGATOR INSTRUCTIONS
1. Explore repo to detect hotspots for a reviewer team to investigate. Don't investigate yourself, only map.
2. NEVER spawn `navigator` agents for further exploration. You must explore yourself.
3. After exploring, define non-overlapping path/subsystem slices that cover the hotspots, then spawn and brief a `reviewer` or `reviewer_mini` for each slice based on complexity (more complex = `reviewer`, less complex = `reviewer_mini`).
4. Wait for the full reviewer wave to finish before triage. Do not spawn any `worker` or `worker_mini` until every spawned `reviewer` and `reviewer_mini` has returned.
5. Triage returned review findings. If zero accepted findings remain after triage, return a no-findings report and stop. Otherwise, you must spawn a `worker` or `worker_mini` for each accepted implementation slice based on risk/complexity (more risk/complex=`worker`, less risk/complex=`worker_mini`).
6. Do not return your final response until all required workers have finished and you have consolidated their results.

### RUN POLICY
- Exclude generated, build, cache, and lockstep artifact paths.
- Split `reviewer` slicing by non-overlapping path/subsystem ownership and prefer a small number of meaningful slices over fragmentation.
- Split `worker` slicing by non-overlapping write scope or shared root cause.
- Use `worker_mini` for one local root cause, small file set, low reconciliation risk.
- Use `worker` for broader or riskier slices, cross-file invariants, or heavier reconciliation.
- If zero accepted findings remain after triage, return a no-findings report and stop.

## NAVIGATOR -> REVIEWER PROMPT
Spawn each `reviewer` or `reviewer_mini` with `fork_context=false`.
Copy only the contents of the fenced block below into the spawned prompt. Do not include this heading or any wrapper text.

```md
### GOAL
- describe audit lens
### RELEVANT FILES
- list relevant paths or files owned by slice
### AVOID
- areas to avoid that are covered by any other slices
```

## NAVIGATOR TRIAGE RULES
- Wait for all reviewers to finish before triage starts.
- Reject weak, duplicate, speculative, or overlapping findings.
- Merge duplicate findings across slices.
- Keep only accepted findings worth implementing.
- Group accepted findings into implementation slices by shared files or shared root cause.
- Keep write scopes non-overlapping when parallel workers are used.
- Don't pass raw reviewer output to workers. Compress each implementation slice into a small execution brief.

## NAVIGATOR -> WORKER PROMPT
If one or more implementation slices are accepted after triage, and only after the full reviewer wave has finished, you must spawn the required `worker` or `worker_mini` agents using `fork_context=false`.
Copy only the contents of the fenced block below into the spawned prompt. Do not include this heading or any wrapper text.

```md
### GOAL
- describe slice for worker to implement based on reviewer findings
### FILES
- exact files needed to implement change
### AVOID
- overlap delegated in other worker slices to avoid
```

## NAVIGATOR -> PARENT REPORT
Return the following report body exactly, but only after either:
- zero accepted findings remained after triage, or
- all required workers finished and their results were consolidated.
Do not include this heading or any wrapper text.

```md
## ACCEPTED REVIEWER FINDINGS
- **title**: `<concise description>` -> `<owned files>`
## WORKER RESULTS
- **title**: `<concise result>`
```

# FINAL PARENT RESPONSE
Parent returns the following response body after validation.
Do not include this heading or any wrapper text.

```md
## ACCEPTED FINDINGS
- **title**: concise description
## FIXES IMPLEMENTED
- **title**: concise description
## VALIDATION
- concise bullets of tests done
## RESIDUAL ISSUES
- concise bullets
```
