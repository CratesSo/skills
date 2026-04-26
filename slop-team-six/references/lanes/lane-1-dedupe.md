# Lane 1: Dedupe and Consolidation

## Primary Goal

Find duplicated code, duplicated data shapes, repeated control flow, or parallel helpers that can be deleted or consolidated with less total code.

This lane is valuable only when consolidation makes the system easier to read in one pass.

## Good Targets

- Copy-pasted functions, branches, tests, config blocks, schema fragments, or constants.
- Multiple helpers that do the same transformation with slightly different names.
- Repeated call-site boilerplate that hides a simple shared operation.
- Parallel module paths that differ only by legacy naming or shallow wrappers.
- Duplicate imports, duplicate validation logic, and duplicate fallback handling.

## Non-Goals

- Do not propose abstractions that add more code, branching, or indirection than they remove.
- Do not merge code that has different ownership, lifecycle, side effects, or failure behavior.
- Do not consolidate tests if duplication is intentionally improving fixture clarity.

## Evidence Required

- Show at least two concrete duplicate sites with file paths and a short similarity explanation.
- Prove the behavior is equivalent enough to consolidate, or state what is still uncertain.
- Estimate the likely code reduction and name the canonical survivor.
- Identify all call sites that would change.
- Note behavior or call-site risk after the consolidation.

## Tools and Signals

Useful signals include duplicate-code scanners, lint duplicate-import warnings, typechecker duplicate/unused diagnostics, and targeted searches for repeated names or literals.

Use evidence from commands such as duplicate-code detection, lint rules, and repo-native checks when available.
If tool output is noisy, report only duplicates that also survive manual inspection.

## False Positives

- Similar code in generated files, migrations, vendored code, snapshots, or fixtures.
- Similar adapters for different external APIs with different failure contracts.
- Test cases duplicated to keep scenarios explicit.
- Separate types with matching fields but different domain meaning.

## Evidence To Gather

- Duplicate cluster summary.
- File paths and symbols involved.
- Canonical survivor candidate.
- Expected code reduction or simplification.
- Risk level and why.
