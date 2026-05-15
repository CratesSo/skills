# Lane 3: AI Slop, Stubs, and Low-Value Comments Cleanup

## Primary Goal

Find low-value comments, stub code, speculative helpers, and scanner-backed clutter that are candidates for deletion after parent triage because they add no behavior, useful context, or active workflow value.

Bias toward deleting confirmed clutter instead of rewriting it. Do not make subjective style-only findings.

## Good Targets

- Comments that restate code, narrate obvious assignments, or explain temporary intent that is demonstrably stale or false.
- Stub functions, fake examples, TODO scaffolds, and probes that are unused and not part of public docs, tests, templates, backlog markers, or workflows.
- Generic helpers, wrappers, config bags, and abstraction layers with one caller when they add no useful boundary, clearer naming, isolated side effect, or ownership boundary.
- Defensive branches for impossible values when the issue is obvious clutter; route deeper error-handling proof to Lane 8.
- Overly broad option objects, unused extension points, and speculative flexibility with no current caller; route package exports or compatibility surfaces to Lane 1 or Lane 2.

## Non-Goals

- Do not treat comments that explain domain rules, surprising behavior, security constraints, performance constraints, operational runbooks, or non-obvious tradeoffs as slop.
- Do not treat intentional test fixtures, examples, docs, templates, public samples, onboarding material, product copy, backlog TODOs, or debug tools still used by workflows as slop.
- Do not replace clutter with prettier clutter.

## Evidence Required

- Show the exact low-value code/comment and why it adds no information, behavior, useful workflow context, or intentional backlog marker.
- Prove whether the target is unused, one-use, redundant, stale, misleading, or scanner-backed noise without relying on scanner output alone.
- Identify behavior impact, package/public surface impact, docs/product-copy impact, and readability/workflow risk.
- Note when deletion depends on another cleanup concern outside this lane.
- Name validation that should catch an incorrect cleanup.

## Tools and Signals

Useful signals include repo slop scanners, lint rules for dead comments, one-caller searches, stale-date checks, and targeted searches for `TODO`, `FIXME`, `stub`, `temporary`, `hack`, `magic`, `just in case`, and `fallback`.

Manual judgment matters; scanner or lint matches are candidates, not proof. A finding must explain why deletion improves the code without removing useful context.

When the hooky slop scanner is available, use `references/tools/slop-scan.md` for the command, scope rules, report order, and missing-tool coverage policy. Treat scanner findings as candidates that still require proof before deletion.

## False Positives

- Real runbooks, public examples, package-exported samples, migration notes, fixtures, templates, product copy, and intentionally verbose tests.
- Comments documenting business rules, security decisions, performance constraints, operational constraints, or irreversible operations.
- Debug hooks used by local workflows.
- Fill-in markers in templates meant for users to complete, and TODO/FIXME markers that represent real accepted backlog or issue references.

## Wave 1 Report Checklist

- Candidate target and location.
- Why it is low-value, stale, misleading, redundant, or scanner-backed noise.
- Proof of no behavior dependency.
- Behavior/readability/workflow risk.
- Related lane if another lane should own it.
- Public/docs/template/product/workflow surface risk.

## Escalate To Parent

- Skip if evidence is subjective and deletion could remove useful context.
- Mark as Lane 1 overlap if the target is primarily unused code, unreferenced files, package exports, or dependency cleanup.
- Mark as Lane 2 overlap if the target is primarily a legacy/fallback/compatibility path or config alias.
- Mark as Lane 8 overlap if the target is primarily defensive error handling.
