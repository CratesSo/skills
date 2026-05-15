# Lane 4: Dedupe and Consolidation

## Primary Goal

Find duplicated code, repeated control flow, parallel helpers, or repeated boilerplate that are candidates for consolidation after parent triage because one canonical implementation would reduce total code, risk, and indirection without merging separate ownership or behavior contracts.

## Good Targets

- Copy-pasted functions, branches, config blocks, non-type schema fragments, or constants with equivalent behavior and matching contracts. Treat duplicated tests as targets only when consolidation would not hide scenario intent.
- Multiple helpers that do the same transformation with slightly different names and no meaningful domain, ownership, lifecycle, or error-contract difference.
- Repeated call-site boilerplate that hides a simple shared operation and can be centralized without obscuring control flow.
- Parallel module paths that differ only by shallow wrappers; route legacy naming or deprecated paths to Lane 2.
- Duplicate imports and duplicate validation logic; route type/schema ownership to Lane 5 and fallback/error-handling duplication to Lane 8 when those are the real proof burden.

## Non-Goals

- Do not treat an abstraction as worthwhile when it adds more code, branching, coupling, or indirection than it removes.
- Do not treat similar code as duplicative when ownership, lifecycle, side effects, performance, ordering, concurrency, observability, or failure behavior differ.
- Do not treat intentionally explicit tests, fixtures, examples, onboarding snippets, public docs, or product samples as cleanup targets when duplication improves scenario clarity or user-facing clarity.

## Evidence Required

- Show at least two concrete duplicate sites with file paths and a short similarity explanation.
- Prove behavior, side effects, error handling, ordering, and public contracts are equivalent enough to consolidate, or state exactly what remains uncertain.
- Estimate likely code reduction and identify the likely canonical survivor for parent triage, including why it should own the shared behavior.
- Identify call sites, ownership boundaries, public/package exports, config surfaces, observability/logging, and docs/examples that would be touched.
- Name validation that should catch an incorrect consolidation.

## Tools and Signals

Useful signals include duplicate-code scanners, lint duplicate-import warnings, targeted searches for repeated names or literals, manual comparison of inputs/outputs/side effects, and call-site/reference searches.

Use evidence from duplicate-code detection, lint rules, and manual call-site review when available.
If tool output is noisy, report only duplicates that also survive manual inspection and have a plausible lower-complexity consolidation path.

For Python, `references/tools/radon.md` can help prioritize complex repeated branches for Lane 4 review when Python files are in scope, but Radon does not prove duplication by itself.

## False Positives

- Similar code in generated files, migrations, vendored code, snapshots, or fixtures.
- Similar adapters for different external APIs with different failure, ordering, performance, lifecycle, observability, retry, or auth contracts.
- Public examples, docs, onboarding snippets, product samples, or test cases duplicated to keep scenarios explicit.
- Separate types or schemas with matching fields but different domain meaning; route true shared-type ownership questions to Lane 5.
- Config blocks that look similar but target different environments, deployment surfaces, package consumers, or security boundaries.

## Wave 1 Report Checklist

- Duplicate cluster summary.
- File paths and symbols involved.
- Behavior, side-effect, ownership, and contract-equivalence evidence plus uncertainty.
- Likely canonical survivor for parent triage.
- Expected code reduction, coupling change, and risk level.
- Public/package/config/docs surface risk.

## Escalate To Parent

- Skip if duplicates are only generated, vendored, fixture, or stylistic.
- Mark as Lane 5 overlap if the duplicate is mostly a type-shape, schema-ownership, parser, serializer, or validation-model problem.
- Mark as Lane 2 overlap if duplicate paths are mainly legacy/canonical-path cleanup.
- Mark as Lane 8 overlap if duplicated fallback, validation, or wrapper code is mainly defensive error handling.
- Mark as Lane 3 overlap if the duplicate is mostly speculative helper clutter or low-value scaffolding.
