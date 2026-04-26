# Lane 8: AI Slop, Stubs, and Low-Value Comments Cleanup

## Primary Goal

Find low-value comments, placeholder code, speculative helpers, over-defensive branches, and AI-looking clutter that can be deleted without changing behavior.

Bias toward deletion over rewriting.

## Good Targets

- Comments that restate code, narrate obvious assignments, or explain temporary intent that is no longer true.
- Stub functions, placeholder files, fake examples, TODO scaffolds, and unused probes.
- Generic helpers, wrappers, config bags, and abstraction layers with one caller.
- Defensive branches for impossible values.
- Overly broad option objects, fallback paths, and unused extension points.

## Non-Goals

- Do not delete comments that explain domain rules, surprising behavior, security constraints, or non-obvious tradeoffs.
- Do not delete intentional test fixtures, examples, docs, or debug tools still used by workflows.
- Do not replace slop with prettier slop.

## Evidence Required

- Show the exact low-value code/comment and why it adds no information or behavior.
- Prove whether the target is unused, one-use, redundant, or misleading.
- Identify behavior impact: ideally none.
- Note when deletion depends on another cleanup concern outside this lane.
- Note behavior or readability risk the parent should consider during triage.

## Tools and Signals

Useful signals include repo slop scanners, lint rules for dead comments, dead-code tools, one-caller searches, and targeted searches for `TODO`, `FIXME`, `stub`, `placeholder`, `temporary`, `hack`, `magic`, `just in case`, and `fallback`.

Good command classes: repo-specific slop scan, lint warnings, dead-code tools, one-caller searches, and targeted exact searches.
Manual judgment matters; tool matches are candidates, not proof.

## False Positives

- Real runbooks, public examples, migration notes, fixtures, and intentionally verbose tests.
- Comments documenting business rules, security decisions, or irreversible operations.
- Debug hooks used by local workflows.
- Placeholders in templates meant for users to fill.

## Evidence To Gather

- Slop target and location.
- Why it is low-value or misleading.
- Proof of no behavior dependency.
- Behavior or readability risk.

## Escalate To Parent

Skip if evidence is subjective and deletion could remove useful context.
