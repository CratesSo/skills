When building each `Wave 1` handoff, provide the full absolute path to `${SKILL_ROOT}/references/roles/wave-1-agents.md` and use the entire template below as their spawn prompt:

# OBJECTIVES

1. Read [absolute path to `${SKILL_ROOT}/references/roles/wave-1-agents.md`]
2. Read [absolute path to each assigned `${SKILL_ROOT}/references/lanes/lane-*.md`]
3. Follow the rules and guidance in `wave-1-agents.md` and `lane-*.md` to execute your goal(s) below.
4. Output a Final Findings Report using the template outlined in this file in the `## FINAL FINDINGS REPORT` section.

## LANE ASSIGNMENT

[Lane n: lane name, or MERGE: Lane n + Lane m]

## GOAL(s)

[describe their task/lane with actionable info and success condition]

- NEVER spawn subagents unless explicitly asked to!
- ALWAYS stay in read-only mode and never implement changes!

## AVAILABLE TOOLS AND COMMANDS

- Tool/check: [real runnable command approved by parent preflight]
  - Use for: [what signal it proves for this lane]
  - Inspect: [specific output, files, warning class, or graph]
- Tool/check: [real runnable command approved by parent preflight]
  - Use for: [what signal it proves for this lane]
  - Inspect: [specific output, files, warning class, or graph]

## RELEVANT FILES

- [files tied to lane scope, each on new line]

## AVOID

- [any overlapping work owned by parallel agents]

## FINAL FINDINGS REPORT

- Write `N/A` only when a field truly does not apply.
- If there are no findings, still fill `## Rollup` and include `## No-Finding Areas`.
- Do not include proposed edits, validation commands, command/search provenance, or parent action hints.

Use the template below for your Final Findings Report:

## Rollup

- **Scope covered:** [files, dirs, symbols, or package areas inspected]
- **Exclusions:** [areas avoided because they were out of scope, owned by another agent, generated, vendor, or risky]

## Findings

### L[n]-F1: [short name]

- **TARGET:** [exact file, directory, symbol, type, dependency, or pattern]
- **EVIDENCE:** [concrete evidence with paths and enough detail for parent triage]
- **WHY IT MATTERS:** [why this is material cleanup signal, not style preference]
- **RISK/UNCERTAINTY:** [what may make this noisy, unsafe, dynamic, public, or ambiguous]
- **CONFIDENCE:** [high | medium | low]

(Repeat as `L[n]-F2`, `L[n]-F3`, etc. for additional findings.)

## No-Finding Areas

- **[area checked]**: [why no material finding was found]

## Blockers

- [missing context, inaccessible files, or unavailable approved tools]
