When building each `Wave 2` handoff, provide the full absolute path to `${SKILL_ROOT}/references/roles/wave-2-agents.md` and use the entire template below as their spawn prompt:

# OBJECTIVE

1. Read [absolute path to `${SKILL_ROOT}/references/roles/wave-2-agents.md`]
2. Execute the goal(s) below following the rules and guidance from `wave-2-agents.md`:

## IMPLEMENTATION BRIEF

- LANE ASSIGNMENT: [Lane n: lane name, or MERGE: Lane n + Lane m]

- RISK NOTES: [known risk and boundaries]

- VALIDATION EXPECTED: [commands you expect after implementation]

## GOAL(s)

[describe the specific edits/tasks the subagent is supposed to implement from the approved triage results with actionable info]

- NEVER spawn subagents!
- Implement only the validated cleanup slice(s).

## RELEVANT FILES

- [files tied to scope, each on new line]

## AVOID

- [any overlapping work owned by parallel subagents]

## FINAL IMPLEMENTATION REPORT

Write `N/A` only when a field truly does not apply.

Report your final implementation results using the template below:

## Rollup

- **Validated brief followed:** [yes/no, with concise reason if no]
- **Scope changed:** [files changed]
- **Residual risks:** [material risks, or N/A]

## Changes

- **[short title]:** [what changed and why it matches the validated brief]

## Validation

- **Run:** [commands run and results]
