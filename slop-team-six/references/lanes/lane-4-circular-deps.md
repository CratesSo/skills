# Lane 4: Circular Dependency Removal

## Primary Goal

Find import, package, module, or build cycles that make dependency flow harder to reason about.

Prefer the smallest structural change that restores a boring one-way dependency.

## Good Targets

- Direct import cycles between files, modules, packages, crates, or projects.
- Barrel files or index modules that create hidden back edges.
- Shared helpers living in a high-level module and imported by lower layers.
- Test utility imports leaking into production paths.
- Dependency cycles that slow builds or force broad initialization ordering.

## Non-Goals

- Do not propose large architectural moves when one import, type move, or helper relocation breaks the cycle.
- Do not break cycles by adding new abstraction layers unless they delete more coupling than they add.
- Do not touch generated or vendored dependency graphs.

## Evidence Required

- Show the cycle path in order.
- Identify the smallest edge that can be removed or inverted.
- Explain current layer direction and desired layer direction.
- Note files likely touched by a fix.
- Name behavior or architecture risk if the cycle is moved instead of removed.

## Tools and Signals

Useful signals include dependency graph tools, import graph reports, compiler cycle errors, package graph tools, and targeted import searches.

Good command classes: `dependency-cruiser`, `madge --circular`, `jdeps`, `deptrac`, `go list`, `cargo tree`, `pydeps`, `grimp`, `include-what-you-use`, native build failures.
Prefer graph output with exact files over vague package-level warnings.

## False Positives

- Type-only imports that do not create runtime cycles in languages that erase them.
- Test-only cycles that do not affect production, unless they block tooling.
- Generated module graphs.
- Dependency duplicates that are not cycles.

## Evidence To Gather

- Cycle path with files/modules.
- Edge causing the cycle.
- Smallest likely fix.
- Risk of moving the edge.

## Escalate To Parent

Skip if no cycle evidence exists.
