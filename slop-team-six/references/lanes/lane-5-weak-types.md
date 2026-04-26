# Lane 5: Weak Type Replacement

## Primary Goal

Find weak, broad, or stringly typed surfaces where stronger local types would remove checks, casts, branching, or ambiguity.

Target weak types only when stronger typing is obvious from nearby source data or call sites.

## Good Targets

- `any`, `unknown`, `interface{}`, `Any`, `Object`, raw maps, untyped dicts, or broad generics.
- Stringly typed states, modes, event names, IDs, or enum-like values.
- Repeated casts, runtime shape checks, and defensive branches caused by missing types.
- Untyped API responses where schemas or adjacent call sites reveal the real shape.
- Suppressed type errors that hide a simple missing type.

## Non-Goals

- Do not invent speculative types.
- Do not strengthen types across public boundaries without compatibility proof.
- Do not replace weak types where the weak boundary intentionally represents arbitrary external input.

## Evidence Required

- Show the weak type and all relevant call sites.
- Identify the stronger type source: schema, constructor, parser, adjacent type, or exhaustive values.
- Show simplification gained by stronger typing.
- List compatibility or migration risk.
- Note type or call-site risk the parent should consider during triage.

## Tools and Signals

Useful signals include strict typecheckers, lint rules against broad types, compiler diagnostics, and targeted searches for casts and suppressions.

Good command classes: `tsc --noEmit`, ESLint type rules, `mypy`, `pyright`, `cargo clippy`, `staticcheck`, `phpstan`, `psalm`, `detekt`, Roslyn analyzers, Swift compiler diagnostics.
Pair diagnostics with call-site tracing.

## False Positives

- External JSON, plugin payloads, CLI args, or user input before validation.
- Type erasure required by framework APIs.
- Generic utilities that are intentionally polymorphic.
- Test helpers that trade precision for fixture speed.

## Evidence To Gather

- Weak type location.
- Stronger type source.
- Call sites and casts affected.
- Expected simplification.
- Compatibility risk.

## Escalate To Parent

Skip if no clear stronger type source exists.
