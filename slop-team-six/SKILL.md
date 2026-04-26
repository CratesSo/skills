---
name: slop-team-six
description: "Run evidence-backed cleanup sweeps using subagents and lane playbooks."
---
Run an execution-first cleanup sweep using subagents: Map the repo, activate relevant cleanup lanes, prove findings before edits, execute validated work in waves, inspect risky work, validate narrowly, and report by lane.

# DEFAULTS

- Default scope is whole repo.
- Default stance is aggressive cleanup with evidence.
- Use repo-native tools when present. If a useful tool is absent, stop and ask permission before installing it.
- Spawn subagents using the role and task references listed below.

## REFERENCE LOADING

- Treat the directory containing this `SKILL.md` as `SKILL_ROOT`.
- Always use absolute reference paths in handoffs. Never expect a spawned subagent to infer `SKILL_ROOT` from relative paths.
- For each spawned subagent, provide its role reference first and its task prompt reference.
- Use these exact role and task references when building subagent handoffs:

- Mapping role: `${SKILL_ROOT}/references/roles/explorer-agents.md`
- Mapping task: `${SKILL_ROOT}/references/wave-0.md`
- Wave 1 role: `${SKILL_ROOT}/references/roles/wave-1-agents.md`
- Wave 1 task: `${SKILL_ROOT}/references/wave-1.md`
- Wave 2 role: `${SKILL_ROOT}/references/roles/wave-2-agents.md`
- Wave 2 task: `${SKILL_ROOT}/references/wave-2.md`

Pass absolute lane reference paths or pasted lane reference content only to `Wave 1` agents. Do not ask `Wave 2` agents to read lane references; pass parent-synthesized validated briefs instead.

## CANONICAL FLOW

1. Use the mapping role and task references to spawn the mapping subagent.
2. While mapping runs, run preflight using the tooling reference.
3. Resolve repo-relevant/useful tool availability before `Wave 1`.
  - If a useful missing tool is needed, stop and ask the user if they want to install it.
4. When mapping agent returns, map cleanup hotspots to lanes and decide which lanes are `ACTIVE`, `SKIP`, or `MERGE`.
5. Use the `Wave 1` role, task, and activated lane references to spawn `Wave 1` subagents.
6. When `Wave 1` is complete, triage its findings into validated implementation briefs.
7. Use the `Wave 2` role and task references to spawn `Wave 2` subagents only for validated implementation using parent-synthesized briefs.
8. Run the parent-owned post-Wave-2 check.
9. Run narrowest useful validation when fully complete.
10. Return the lane-grouped final report.

## PREFLIGHT

`preflight` means finding:

- languages in use
- workspace layout
- type systems and compiler configs
- available analysis tools by language, using `${SKILL_ROOT}/references/tooling.md` as the command matrix
- repo-native test, lint, or check commands
- generated, vendor, cache, dist, build, coverage, lockfile, and fixture areas to exclude
- available tools and real runnable commands for each active lane

`Wave 1` must not start until tool availability is known and each `Wave 1` handoff lists usable commands.

## EIGHT LANES

These are the eight cleanup lanes. Keep in this order:

1. Dedupe and consolidation
2. Shared type consolidation
3. Unused code discovery and removal
4. Circular dependency removal
5. Weak type replacement
6. Unnecessary defensive error handling removal
7. Legacy, fallback, and deprecated path removal
8. AI slop, stubs, and low-value comments cleanup

Don't force all eight lanes to edit (execution is adaptive).

Each lane must become:

- `ACTIVE`: enough evidence to investigate in Wave 1
- `SKIP`: repo does not meaningfully support lane
- `MERGE`: lane overlaps another lane enough that one subagent should own both

## LANE REFERENCES

Pass lane references to `Wave 1` subagents as discovery playbooks for lanes marked `ACTIVE` or `MERGE`:

- Lane 1: `${SKILL_ROOT}/references/lanes/lane-1-dedupe.md`
- Lane 2: `${SKILL_ROOT}/references/lanes/lane-2-shared-types.md`
- Lane 3: `${SKILL_ROOT}/references/lanes/lane-3-unused-code.md`
- Lane 4: `${SKILL_ROOT}/references/lanes/lane-4-circular-deps.md`
- Lane 5: `${SKILL_ROOT}/references/lanes/lane-5-weak-types.md`
- Lane 6: `${SKILL_ROOT}/references/lanes/lane-6-defensive-errors.md`
- Lane 7: `${SKILL_ROOT}/references/lanes/lane-7-legacy-fallbacks.md`
- Lane 8: `${SKILL_ROOT}/references/lanes/lane-8-ai-slop.md`

If a lane is `MERGE`, list every lane in the group and every lane reference absolute path the subagent must read.

## TRIAGE

You own triage. Never pass raw `Wave 1` output straight into edits.

Before `Wave 2`:

- Merge duplicate findings.
- Reject weak or overlapping findings.
- Collapse related lanes when they share files or root cause.
- Decide which work should run in parallel and keep write scopes non-overlapping.
- Convert accepted findings into validated implementation briefs with exact files, edits, lane constraints, risks, and validation expectations.

## POST-WAVE-2 CHECK

After `Wave 2` finishes, the you must:

- Inspect changed files against the validated implementation briefs.
- Confirm lane constraints were followed.
- Confirm validation state and skipped checks.
- Record residual risks for the final report.

## FINAL RESPONSE

Return results grouped by lane using the template below:

## PREFLIGHT

- **Languages:** [list languages]
- **Tools Used:** [list tools]
- **Exclusions:** [list exclusions]

## LANE STATUS

- **Lane 1:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 2:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 3:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 4:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 5:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 6:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 7:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 8:** `ACTIVE` | `SKIP` | `MERGE`

## CHANGES BY LANE

### Lane n:

- **EVIDENCE:** [brief summary]
- **EDITS:** [brief summary]
- **VALIDATION:** [brief summary]
- **RISKS:** [brief summary]

## RESIDUAL

- [blockers, deferred lanes, or remaining risks]
