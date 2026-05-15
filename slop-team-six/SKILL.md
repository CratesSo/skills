---
name: slop-team-six
description: "Run evidence-backed cleanup sweeps that map lanes, edit in waves, and validate narrowly."
---
Run execution-first cleanup sweep. Scan repo, activate relevant cleanup lanes, prove relevance before edits, execute in evidence, implementation, review/validation, and final reporting waves, then report by lane. Follow instructions below carefully and exactly:

# DEFAULTS

- Default scope is whole repo.
- Default stance is aggressive cleanup with evidence.
- Use repo-native tools when present. If a useful analysis tool is absent, install shared tools before spawning agents or approve lane-scoped install commands in the relevant handoff.
- Always spawn agents with `fork_context: false`.

## REFERENCE LOADING

- Read spawned-agent prompt references only before spawning that agent.
- Read `references/tooling.md` during preflight and build a parent-owned tool inventory before Wave 1 starts.
- Pass lane reference paths to Wave 1 agents; do not load lane references in the parent unless needed to resolve ambiguity.
- Do not ask Wave 2 or reviewer agents to read lane references. Pass them parent-synthesized validated briefs instead.

## CANONICAL FLOW

1. Read `references/explorer_deep.md`, then spawn `explorer_deep` agent to map repo structure, exclusions, tool clues, and cleanup hotspots.
2. While `explorer_deep` works, run `preflight` check and read `references/tooling.md`.
3. When `explorer_deep` is done, map its cleanup hotspots to lanes and decide which lanes are active, skipped, or merged.
4. Resolve tool availability before Wave 1; install useful shared tools when needed, or explicitly approve lane-scoped install commands in the relevant Wave 1 handoff.
5. Read `references/wave1-cleanup.md`, then run `Wave 1` read-only cleanup discovery in numbered lane batches using the lane table below.
6. After each numbered `Wave 1` batch finishes, triage results, merge overlaps, and reshape remaining batches. After all accepted `Wave 1` findings are triaged, convert them into validated implementation briefs.
7. When accepted `Wave 1` findings are converted into implementation briefs, read `references/wave2-worker.md`, then spawn `Wave 2` worker agents only for validated implementation using the parent-synthesized brief.
8. Run `Wave 3` review and validation: collect worker validation, run missing lane-relevant checks, and prepare review context for changed scope.
9. For higher-risk completed lanes in `Wave 3`, read `references/reviewer.md`, then spawn `reviewer_heavy` with changed files, accepted Wave 1 proof, validation state, and parent-synthesized lane constraints.
10. Triage reviewer findings. If follow-up edits are needed, send a new validated implementation brief with accepted proof to a `Wave 2` worker, then rerun the relevant `Wave 3` validation/review loop.
11. Run `Wave 4` final reporting: confirm no unresolved blockers are hidden, reconcile lane statuses, and record final validation, skipped validation, risks, and leftovers.
12. Return track-by-track summary with risks and leftovers.

## PREFLIGHT

`preflight` means finding:

- languages in use
- workspace layout
- type systems and compiler configs
- available analysis tools by language, using `references/tooling.md` as the command matrix
- repo-native test, lint, or check commands
- obvious generated, vendor, cache, dist, build, coverage, and lockfile areas to exclude
- available tools and real runnable commands for each active lane
- missing-but-useful tools and any parent-approved lane-scoped install commands
- relevant lane-specific tool references under `references/tools/`

Read `references/tooling.md` during preflight. `Wave 1` must not start until tool availability is known and each `Wave 1` handoff lists usable commands plus any parent-approved install commands.

## LANES AND WAVE 1 BATCHES

Don't force all eight lanes to edit. Before `Wave 1`, assign each lane one state: `ACTIVE`, `SKIP`, or `MERGE`.

| Lane | Batch | Cleanup category | Reference |
| --- | --- | --- | --- |
| 1 | `Wave 1.1` | Unused code discovery and removal | `references/lanes/lane-1-unused-code.md` |
| 2 | `Wave 1.1` | Legacy, fallback, and deprecated path removal | `references/lanes/lane-2-legacy-fallbacks.md` |
| 3 | `Wave 1.1` | AI slop, stubs, and low-value comments cleanup | `references/lanes/lane-3-ai-slop.md` |
| 4 | `Wave 1.2` | Dedupe and consolidation | `references/lanes/lane-4-dedupe.md` |
| 5 | `Wave 1.2` | Shared type consolidation | `references/lanes/lane-5-shared-types.md` |
| 6 | `Wave 1.2` | Circular dependency removal | `references/lanes/lane-6-circular-deps.md` |
| 7 | `Wave 1.3` | Weak type replacement | `references/lanes/lane-7-weak-types.md` |
| 8 | `Wave 1.3` | Unnecessary defensive error handling removal | `references/lanes/lane-8-defensive-errors.md` |

Run `Wave 1` discovery by numbered batch unless the scope is tiny enough that overlap is obviously low. Before spawning a `Wave 1` agent, assign it exactly one `ACTIVE` lane or one explicit `MERGE` group. If a lane is `MERGE`, list every lane in the group and every lane reference path the agent must read.

Pass only the assigned lane reference(s) plus relevant tool references to each `Wave 1` agent. Never pass the full `references/tooling.md` matrix to subagents.

After each numbered discovery batch, triage findings, merge overlaps, and skip or reshape later lane assignments based on accepted evidence. Only run lanes in the same batch in parallel when their file scopes are unlikely to overlap.

## WAVE 1: EVIDENCE

For each `ACTIVE` or `MERGE` lane assignment in the current numbered discovery batch, spawn a read-only `cleanup` agent to gather evidence-backed findings for parent triage.

Use `references/tooling.md` to build the available command list for each `Wave 1` handoff. If a needed tool is absent, install it before spawning the lane agent.

## TRIAGE

You own triage. Never pass raw `Wave 1` output straight into edits.

Before `Wave 2`:

- Merge duplicate findings.
- Reject weak or overlapping findings.
- Collapse related lanes when they share the same files or root cause.
- Decide which lanes need `worker_mini` versus `worker`.
- Convert accepted Wave 1 findings into a validated implementation brief with exact files, edits, accepted evidence/proof, allowed write scope, disallowed files/work, lane constraints, risks, and validation expectations.

## WAVE 2: EXECUTION

`Wave 2` implements only validated cleanup work:

- Use `worker_mini` for narrow, obvious cleanup slices.
- Use `worker` for cross-file or high risk cleanup slices.
- Keep write scopes non-overlapping when parallel workers run.

## WAVE 3: REVIEW AND VALIDATION

`Wave 3` is parent-owned review and validation after `Wave 2` edits:

- Collect worker-reported validation and changed files.
- Run missing lane-relevant checks before high-risk review.
- Use repo-native tests, compilers, linters, and analyzers.
- Give reviewer agents the changed files, accepted Wave 1 proof, and validation already run; do not ask reviewers to infer validation state.
- Keep reviewer agents read-only.
- If review requires edits, return to `Wave 2` with a new validated implementation brief and accepted proof, then rerun the relevant `Wave 3` checks.
- Run final narrow validation after all review-driven follow-up edits are complete.

## WAVE 4: FINAL REPORTING

`Wave 4` is parent-owned closeout after `Wave 3` passes or all blockers are explicit:

- Do not spawn new agents or make new edits in `Wave 4`. If a new blocker needs edits, return to `Wave 2`, then rerun the relevant `Wave 3` checks.
- Confirm all worker and reviewer agents have completed and their outputs were triaged.
- Reconcile every lane to exactly one final status from the final response template.
- Report validation actually run, validation skipped or unavailable, and any residual risk without overclaiming.
- Keep merged lanes visible in final reporting even when one implementation brief covered multiple lanes.

Keep final response concise and return results grouped by lane, even if multiple lanes merged during execution, using the template inside the fenced block below:

```md
## PREFLIGHT

- **Languages:** [list languages]
- **Tools Used:** [list tools]
- **Exclusions:** [list exclusions]

## LANE STATUS

Use one of: `SKIPPED`, `INVESTIGATED`, `ACCEPTED`, `IMPLEMENTED`, `DEFERRED`, `MERGED INTO LANE n`.

- **Lane 1:** [status]
- **Lane 2:** [status]
- **Lane 3:** [status]
- **Lane 4:** [status]
- **Lane 5:** [status]
- **Lane 6:** [status]
- **Lane 7:** [status]
- **Lane 8:** [status]

## CHANGES BY LANE

### Lane n:

- **EVIDENCE:** [brief summary]
- **EDITS:** [brief summary]
- **VALIDATION:** [brief summary]
- **RISKS:** [brief summary]

## RESIDUAL

- any blockers, deferred lanes, or remaining risks
```
