# Lane 7: Weak Type Replacement

## Primary Goal

Find weak, broad, or stringly typed surfaces that are candidates for parent-triaged strengthening because stronger local types would remove checks, casts, branching, or ambiguity without narrowing valid external input.

Target weak types only when stronger typing is obvious from nearby source data, schemas, constructors, parsers, exhaustive values, or call sites. Prefer local, boundary-owned types over broad shared models unless shared ownership is already clear.

## Good Targets

- `any`, `unknown`, `interface{}`, `Any`, `Object`, raw maps, untyped dicts, or broad generics after the value has crossed a validation or construction boundary.
- Stringly typed states, modes, event names, IDs, or enum-like values where exhaustive values are visible and owned by the repo.
- Repeated casts, runtime shape checks, and defensive branches caused by missing types.
- Untyped API responses after validation/parsing when owned schemas, generated types, or adjacent call sites reveal the real shape.
- Suppressed type errors that hide a simple missing type and do not mask a broader design, third-party typing, version skew, or dependency issue.

## Non-Goals

- Do not invent speculative types, enum values, object shapes, or generic constraints.
- Do not strengthen types across public, package-exported, plugin, generated, vendor, wire, docs, persistence, analytics/event, or third-party SDK boundaries without compatibility proof.
- Do not replace weak types where the weak boundary intentionally represents arbitrary external input, partially trusted data, feature-flag payloads, env vars, CLI args, plugin data, or user input before validation.
- Do not introduce a shared type when a local type, parser-owned type, schema-owned type, or generated type would keep ownership clearer.

## Evidence Required

- Show the weak type and all relevant call sites, casts, suppressions, shape checks, branches, validators, and construction points.
- Identify the stronger type source: owned schema, generated type, constructor, parser, adjacent type, exhaustive values, or validated boundary.
- Show simplification likely gained by stronger typing, such as removed casts, suppressions, shape checks, branches, impossible-state guards, or narrower APIs.
- List compatibility, migration, public API, package-export, wire, docs, persistence, generated-code, analytics/event, third-party typing, or dependency-direction risk.
- Name typecheck/compiler validation that should catch an incorrect strengthening.

## Tools and Signals

Useful signals include strict typecheckers, lint rules against broad types, compiler diagnostics, schema/parser references, generated type references, and targeted searches for casts and suppressions.

Pair diagnostics with call-site tracing, construction-point tracing, and boundary classification.

For Python complexity and maintainability hotspots, use `references/tools/radon.md` as a supporting signal when Python files are in scope. Treat Radon output as prioritization evidence for weakly typed, branch-heavy, or ambiguous surfaces; it is not proof that a type replacement is correct.

## False Positives

- External JSON, plugin payloads, CLI args, env vars, feature-flag payloads, partially trusted data, or user input before validation.
- Type erasure required by framework APIs.
- Generic utilities that are intentionally polymorphic and have tests or call sites proving that flexibility is used.
- Public, package-exported, generated, vendor-owned, wire, analytics/event, third-party SDK, or persistence contract types.
- Test helpers that trade precision for fixture speed without hiding production type risk.

## Wave 1 Report Checklist

- Weak type location.
- Stronger type source, validated boundary, and ownership.
- Call sites, casts, suppressions, shape checks, or impossible-state guards affected.
- Expected simplification and uncertainty.
- Boundary classification, dependency-direction risk, compatibility risk, and validation expectation.

## Escalate To Parent

- Skip if no clear stronger type source exists.
- Mark as Lane 5 overlap if the candidate requires creating or using a shared type.
- Mark as Lane 8 overlap if weak typing exists mainly to support defensive checks or impossible-state guards.
- Mark as Lane 3 overlap if the weak type is only unused scaffolding or speculative clutter.
- Mark as Lane 6 overlap if strengthening the type would change import direction or create/break a cycle.
