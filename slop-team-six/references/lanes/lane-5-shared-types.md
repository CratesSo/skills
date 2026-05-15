# Lane 5: Shared Type Consolidation

## Primary Goal

Find duplicate or drifting type definitions that are candidates for parent-triaged consolidation because one canonical type would make ownership clearer and call sites simpler without collapsing distinct boundary contracts.

Focus on type shapes that cross module boundaries or create repeated mapping, parsing, serialization, or validation code inside the repo-owned domain.

## Good Targets

- Duplicate DTOs, schemas, interfaces, structs, dataclasses, enums, or validation models tied to the same repo-owned domain concept.
- Types that mirror API payloads but drift in optional fields or naming after the external boundary is parsed or adapted into an internal model.
- Local type aliases that hide the same domain concept in several modules and make call sites or validation less clear.
- Conversion functions that only translate between near-identical shapes after proving they are not intentionally separating external DTOs from internal domain models.
- Repeated parser, serializer, or validation declarations tied to the same owned domain concept and same boundary.

## Non-Goals

- Do not treat shared types as correct for concepts that only look structurally similar.
- Do not cross bounded contexts, package boundaries, service boundaries, or layering boundaries without evidence that ownership should be shared.
- Do not collapse request, response, persistence, wire, generated, vendor, third-party SDK, analytics, event, and view models when their constraints, lifecycles, ownership, privacy, or compatibility requirements differ.

## Evidence Required

- List matching fields, variants, schema members, invariants, validation rules, defaults, nullability, optionality, and meaningful differences.
- Identify current owners and the likely canonical owner for parent triage, including the module/package that should own imports.
- Show call sites or conversion code that would simplify, and confirm the conversion is not preserving a real boundary.
- Identify downstream compile/typecheck risks, import/dependency direction changes, layering changes, and whether the move would create or break a cycle.
- Note compatibility concerns for public API, database, wire, persisted, generated, vendor-pinned, analytics/event, docs, or package-exported shapes.

## Tools and Signals

Useful signals include typechecker diagnostics, schema generators, API contract files, duplicate struct/interface searches, conversion helpers, validation schemas, and import graph checks.

Prefer language-native type and schema tools over text similarity alone. Matching fields are a lead, not proof of shared meaning.
Searches should include type names, shared field groups, schema names, and conversion helper names such as `from`, `to`, `map`, `serialize`, and `parse`.

## False Positives

- Same fields with different invariants, defaults, nullability, lifecycle, ownership, privacy, or semantic meaning.
- External API, generated, vendor-pinned, analytics/event, or package-exported types that must preserve a contract.
- Persistence, wire, request, response, event, analytics, and view models that intentionally differ.
- Schema duplicates generated from the same source of truth where the generator, not hand consolidation, owns output.
- Test-only types or builders that keep fixtures readable.
- Boundary DTOs kept separate from internal domain models for validation, privacy, compatibility, or anti-corruption-layer reasons.

## Wave 1 Report Checklist

- Candidate types and owning modules.
- Field, variant, schema, validation-rule, default/nullability, and invariant overlap.
- Conversion or call-site simplification evidence.
- Likely canonical owner for parent triage.
- Import/dependency direction, layering, and cycle risk.
- Public, package-exported, wire, generated, vendor, analytics/event, docs, or persistence compatibility risk.

## Escalate To Parent

- Skip if shared ownership would be less clear than the duplication.
- Mark as Lane 4 overlap if the problem is mostly duplicated behavior rather than type ownership.
- Mark as Lane 7 overlap if weak typing exists because a canonical type is missing.
- Mark as Lane 6 overlap if moving the canonical type would change dependency direction or break/create a cycle.
- Mark as Lane 2 overlap if duplicate types exist only to support deprecated compatibility surfaces.
