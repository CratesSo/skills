# Lane 2: Shared Type Consolidation

## Primary Goal

Find duplicate or drifting type definitions where one canonical type would make ownership clearer and call sites simpler.

Focus on type shapes that cross module boundaries or create repeated mapping code.

## Good Targets

- Duplicate DTOs, schemas, interfaces, structs, dataclasses, enums, or validation models.
- Types that mirror API payloads but drift in optional fields or naming.
- Local type aliases that hide the same domain concept in several modules.
- Conversion functions that only translate between near-identical shapes.
- Repeated parser, serializer, or validation declarations.

## Non-Goals

- Do not propose shared types for concepts that only look similar.
- Do not cross bounded contexts without evidence that ownership should be shared.
- Do not collapse request, persistence, and view models when their constraints differ.

## Evidence Required

- List matching fields, variants, or schema members.
- Identify the current owner and the most natural canonical owner.
- Show call sites or conversion code that would simplify.
- Identify downstream compile/typecheck risks.
- Note compatibility concerns for public API, database, wire, or persisted shapes.

## Tools and Signals

Useful signals include typechecker diagnostics, schema generators, API contract files, duplicate struct/interface searches, and compiler unused warnings after consolidation.

Prefer language-native type tools over text similarity alone.
Searches should include type names, shared field groups, and conversion helper names such as `from`, `to`, `map`, `serialize`, and `parse`.

## False Positives

- Same fields with different invariants or lifetimes.
- External API types pinned to vendor contracts.
- Persistence models that intentionally differ from input/output types.
- Test-only types or builders that keep fixtures readable.

## Evidence To Gather

- Candidate types and owning modules.
- Field or variant overlap.
- Simplified call sites or deleted conversions.
- Natural canonical owner evidence.
- Public or persistence compatibility risk.

## Escalate To Parent

Skip if shared ownership would be less clear than the duplication.
