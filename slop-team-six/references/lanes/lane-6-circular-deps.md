# Lane 6: Circular Dependency Removal

## Primary Goal

Find import, package, module, or build cycles that are candidates for parent-triaged cleanup because they make dependency flow harder to reason about or create real runtime/build/tooling risk.

Prefer evidence for the smallest structural change that would restore a boring one-way dependency without creating an unowned generic shared module.

## Good Targets

- Direct import cycles between files, modules, packages, crates, or projects.
- Barrel files or index modules that create hidden back edges.
- Shared helpers or types living in a high-level module and imported by lower layers, when a lower-level owner or existing shared owner is clear.
- Test utility imports leaking into production paths, or test cycles that block tooling in a material way.
- Runtime initialization cycles, build cycles, package cycles, or project-reference cycles that slow builds or force broad initialization ordering.

## Non-Goals

- Do not treat large architectural moves as justified when one import, type move, dependency inversion, or helper relocation would likely break the cycle.
- Do not treat new abstraction layers or generic `shared` modules as justified unless they delete more coupling than they add and have a clear owner.
- Do not treat generated, vendored, fixture, build-output, lockfile-only, or tool-only dependency graphs as cleanup targets unless they block real repo tooling.
- Do not move public/package-exported APIs or plugin entrypoints just to satisfy an internal graph unless compatibility impact is understood.

## Evidence Required

- Show the cycle path in order, including whether it is runtime, type-only, test-only, build-time, package-level, or project-reference level, and whether the language/toolchain treats it as a real cycle.
- Identify the smallest edge that appears removable, movable, or invertible for parent triage, and why that edge is lower risk than alternative edges.
- Explain current layer direction, desired layer direction, and why that candidate direction matches existing ownership and module boundaries.
- Note files likely touched, public/package/plugin surfaces involved, initialization side effects, and why that scope is small enough.
- Check whether moving the edge would create another cycle, worsen layering, or cross a boundary owned by Lane 5, Lane 4, or Lane 2.
- Name validation that should prove the cycle is gone and behavior still works.

## Tools and Signals

Useful signals include dependency graph tools, import graph reports, compiler cycle errors, project-reference reports, package graph tools, initialization traces, and targeted import searches.

Prefer graph output with exact files and edge direction over vague package-level warnings. For type-only cycles, verify whether the build/runtime actually cares before escalating.

## False Positives

- Type-only imports that do not create runtime, build, lint, or tooling cycles in languages that erase them.
- Test-only cycles that do not affect production, unless they block tooling, slow meaningful validation, or leak into production paths.
- Generated, vendored, fixture, build-output, lockfile-only, or tool-only module graphs.
- Dependency duplicates, version conflicts, or peer-dependency warnings that are not cycles.
- Public/package-exported barrels that intentionally define supported API shape without creating runtime, build, lint, or tooling cycles.

## Wave 1 Report Checklist

- Cycle path with files/modules and edge direction.
- Cycle type: runtime, type-only, test-only, build-time, package-level, or project-reference level.
- Smallest likely edge to remove, move, or invert for parent triage.
- Ownership, public-surface, initialization, layering, and new-cycle risk of moving the edge.

## Escalate To Parent

- Skip if no concrete cycle evidence exists.
- Mark as Lane 5 overlap if moving a shared type or schema is the smallest way to break the cycle.
- Mark as Lane 4 overlap if duplicate helpers are the root cause of the cycle.
- Mark as Lane 2 overlap if the cycle exists only through a legacy, deprecated, or compatibility path.
- Mark as Lane 3 overlap if the cycle exists only through speculative scaffolding or low-value helper clutter.
