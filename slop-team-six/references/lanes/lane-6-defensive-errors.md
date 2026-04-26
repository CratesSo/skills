# Lane 6: Unnecessary Defensive Error Handling Removal

## Primary Goal

Find defensive error handling that swallows failures, guards impossible scenarios, duplicates upstream guarantees, or hides useful errors.

Keep boundaries that intentionally sanitize external input, convert external errors, or recover from expected failures.

## Good Targets

- Empty catches, broad catches, ignored errors, redundant wrappers, and rethrow-only handlers.
- Guards for states made impossible by types, parsers, constructors, or earlier checks.
- Fallback returns that mask failed writes, uploads, payments, migrations, or persistence.
- Duplicate validation after a canonical validator already ran.
- Error conversion layers that remove context without adding a useful domain boundary.

## Non-Goals

- Do not remove error handling at user input, network, filesystem, database, process, or third-party boundaries without proof.
- Do not weaken observability or cleanup behavior.
- Do not remove retry/recovery behavior that is part of product semantics.

## Evidence Required

- Show the defensive block and the upstream guarantee or duplicate handling.
- Prove the guarded scenario is impossible, redundant, or harmful.
- Identify whether the boundary is internal or external.
- Explain what error will surface after cleanup.
- Note boundary risk the parent should consider during triage.

## Tools and Signals

Useful signals include lint rules for broad/empty catches, compiler exhaustiveness checks, typechecker narrowing, tests around error paths, and targeted searches for `catch`, `except`, `panic`, `recover`, `try`, `fallback`, `ignore`, and `TODO`.

Good command classes: ESLint `no-useless-catch`/`no-empty`, Ruff `TRY`/`B` rules, `staticcheck`, clippy, shellcheck, compiler exhaustiveness warnings.

## False Positives

- Intentional error normalization at API, CLI, worker, or UI boundaries.
- Cleanup in `finally`/defer/drop blocks.
- Security-sensitive input sanitization.
- Idempotent retries and expected missing-resource handling.

## Evidence To Gather

- Defensive block location.
- Proof it is impossible, redundant, or harmful.
- Boundary classification: internal or external.
- Error behavior after cleanup.
- Behavior/risk after cleanup.

## Escalate To Parent

Skip if impossible-state proof is weak.
