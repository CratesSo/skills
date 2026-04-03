---
name: strike-team
description: Run a staged repo-wide audit-and-fix operation using `explorer`, a bounded-parallel `reviewer` team, and bounded-parallel `worker` team under your control.
---
# Run Repo-wide Audit That Lands Fixes

## === CANONICAL FLOW ===

1. Lock audit lens.
2. Map repo with one or more `explorer` agents.
3. Slice reviewer work by subsystem/path.
4. Triage `reviewer` findings yourself.
5. Delegate coherent fix slices to `worker` agents.
6. Run narrowest relevant validation.
7. Return concise audit-and-change summary.

## === CONSTRAINTS ===

- If user already gave audit lens, use it.
- If not, ask for audit lens before continuing.
- Default scope is whole repo.
- Exclude obvious generated, vendor, cache, dist, build, coverage, and lockstep artifact areas unless asked to include.
- Default end state is implemented fixes + validation.

## === USAGE GUIDELINES ===

- NEVER use this skill for:
  - single-file review
  - one-off debugging
  - architecture brainstorming without code changes
  - audit-only requests where no edits should be made

## === ORCHESTRATION RULES ===

- YOU own the run end-to-end.
- Subagents report upward only.
- ALWAYS spawn subagents with `fork_context: false`.
- ALWAYS start subagent including `# COMPLETE TASK WITHOUT USING SUBAGENTS!`
- YOU are the sole triage authority before edits begin.
- Use bounded parallelism for `reviewer` and `worker` waves.
  - NEVER fan out everything at once.
- Size `reviewer` and `worker` teams dynamically from repo shape and finding volume.
- Group implementation work by coherent fix slice with shared files or shared root cause.
  - NEVER do one worker per finding unless clearly the cleanest cut.

## === PHASE 1: EXPLORER PASS ===

Spawn one or more `explorer` agents first, based on repo size and clarity, to map structure relevant to audit lens.

### Explorer Brief:
- Read-only.
- Map repo structure relevant to requested audit lens.
- Identify major subsystems, entry points, risky hotspots, and likely reviewer slice boundaries.
- Call out obvious generated/vendor/build areas to exclude from review by default.
- Return concise report for you to slice on.

- If first `explorer` report is incomplete because repo is large or ambiguous, you may spawn additional `explorer` for unclear areas after reviewing first report.

## === PHASE 2: REVIEWER SLICING ===

### After `explorer` Report(s):
- Derive `reviewer` assignments by subsystem/path, not generic themes.
- Keep slices coherent and non-overlapping where possible.
- Prefer small number of meaningful slices over noisy fragmentation.
- Spawn `reviewer` agents in bounded parallel.

### Reviewer Brief:
- Read-only.
- Review only assigned slice.
- Optimize for user-provided audit lens.
- Findings must be evidence-based, concrete, and scoped to assigned area.
- Call out regressions, correctness issues, security/safety risks, architectural faults, or other lens-relevant problems.

## === PHASE 3: TRIAGE ===

### When `reviewer` Report(s) Return, You Must:
- Merge duplicates.
- Resolve overlaps or conflicts.
- Reject weak, redundant, or low-value findings.
- Choose final accepted finding set.
- Convert accepted findings to coherent implementation slices.
- NEVER pass raw reviewer output straight to `worker` agents.

### Implementation Slices Should:
- Bundle related findings in same area when sharing files or root cause.
- Stay narrow enough for a single `worker` to complete cleanly.
- Avoid overlapping write scopes when parallel workers are used.

## === PHASE 4: WORKER EXECUTION ===

Spawn `worker` agents in bounded parallel for accepted implementation slices.

### Worker Brief:
- Implement only assigned slice.

### Parent Agent Responsibilities During `worker` Execution:
- Avoid overlapping write assignments.
- Integrate `worker` results into one coherent outcome.
- Make any final direct edits needed to reconcile adjacent changes.

## === PHASE 5: VALIDATION ===

### After `worker` Edits Land:
- Run narrowest checks to cover changes.
  - Prefer focused tests, targeted builds, or narrow lint/typecheck commands.
  - Avoid broad repo-wide validation unless fixes force it

## === FINAL RESPONSE ===

Return concise report template:

<template>
## ACCEPTED FINDINGS
- **<title>**: <concise_description>
## FIXES IMPLEMENTED
- **<title>**: <concise_description>
## VALIDATION
- <concise_bullets>
## RESIDUAL ISSUES
- <concise_bullets>
</template>
