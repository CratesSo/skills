# Lane 8: Unnecessary Defensive Error Handling Removal

## Primary Goal

Find defensive error handling that is a candidate for parent-triaged cleanup because it swallows failures, guards impossible scenarios, duplicates upstream guarantees, hides useful errors, or makes failures less observable.

Keep boundaries that intentionally sanitize external input, convert external errors, preserve observability, clean up resources, enforce security, maintain idempotency, or recover from expected failures.

## Good Targets

- Empty catches, broad catches, ignored errors, redundant wrappers, and rethrow-only handlers after proving they are not preserving cleanup, retry, audit, telemetry, or boundary semantics.
- Guards for states made impossible by types, parsers, constructors, exhaustive enums, earlier checks, or canonical validators.
- Fallback returns that mask failed writes, uploads, payments, migrations, persistence, security checks, or user-visible operations.
- Duplicate validation after a canonical validator already ran and no boundary crossing occurred afterward.
- Error conversion layers that remove context without adding a useful domain, API, security, package, or user-facing boundary.

## Non-Goals

- Do not treat error handling at user input, network, filesystem, database, process, public API, package-exported, plugin, worker, UI, security, payment, migration, persistence, or third-party boundaries as removable without proof.
- Do not weaken observability, cleanup behavior, context preservation, audit logging, telemetry, alerting, or resource release.
- Do not treat retry/recovery behavior, idempotency, normalization, rate-limit handling, partial-failure handling, or fallback UX as removable when it is part of product semantics.
- Do not treat documented error shapes, error codes, compatibility behavior, or client-facing messages as removable without support-window proof.

## Evidence Required

- Show the defensive block and the upstream guarantee, duplicate handling, type/parser guarantee, exhaustive value set, canonical validator, or impossible-state proof.
- Prove the guarded scenario is impossible, redundant, harmful, context-destroying, or less observable than the natural failure path.
- Identify whether the boundary is internal, public, package-exported, external, security-sensitive, user-facing, documented, retry/idempotency-related, observability-related, or resource-cleanup related.
- Explain what error or behavior would surface after parent-approved cleanup, including whether observability, retries, audit logs, user-facing messages, and resource cleanup remain intact.
- Name validation that should catch an incorrect cleanup.

## Tools and Signals

Useful signals include lint rules for broad/empty catches, compiler exhaustiveness checks, typechecker narrowing, tests around error paths, public error contract docs, telemetry/audit usage, and targeted searches for `catch`, `except`, `panic`, `recover`, `try`, `fallback`, `ignore`, `TODO`, `finally`, `defer`, and cleanup handlers.

## False Positives

- Intentional error normalization at API, CLI, package-exported, worker, UI, plugin, security, payment, persistence, migration, or third-party boundaries.
- Cleanup in `finally`, `defer`, `drop`, context-manager, transaction rollback, lock release, or resource-release blocks.
- Security-sensitive input sanitization, redaction, authorization handling, or audit logging.
- Documented error shapes, compatibility behavior, and support-window fallbacks.
- Idempotent retries, fallback UX, rate-limit handling, partial-failure handling, and expected missing-resource handling.

## Wave 1 Report Checklist

- Defensive block location.
- Proof it is impossible, redundant, harmful, context-destroying, or less observable than the natural failure path.
- Boundary classification and cleanup/observability/resource-release/retry/security role.
- Simplification signal for parent triage.
- Behavior, public contract, observability, recovery, and risk after cleanup.

## Escalate To Parent

- Skip if impossible-state or redundancy proof is weak.
- Mark as Lane 7 overlap if the defensive block exists mainly because a type is too weak or missing exhaustive values.
- Mark as Lane 2 overlap if the fallback is mainly a legacy compatibility path, documented compatibility behavior, or support-window issue.
- Mark as Lane 3 overlap if the block is unused scaffolding or speculative clutter.
- Mark as Lane 5 overlap if duplicate validators or parsers indicate a missing canonical type/schema owner.
