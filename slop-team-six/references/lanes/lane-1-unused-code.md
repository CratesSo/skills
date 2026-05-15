# Lane 1: Unused Code Discovery and Removal

## Primary Goal

Find code, exports, dependencies, files, or configuration that appear unused and are candidates for deletion after parent triage. Prefer private/internal targets; public API and package-surface targets need stronger source-of-truth proof or human confirmation.

## Good Targets

- Unreferenced functions, classes, components, constants, files, exports, or package dependencies.
- Dead test helpers, scripts, feature modules, old entrypoints, and stale config that are not referenced by repo tooling or runtime loaders. Treat tests as removable only when they exclusively cover confirmed dead code or obsolete behavior.
- Unused imports, variables, parameters, and private methods.
- Dependencies present in manifests but absent from imports, runtime loading, package-manager scripts, plugin discovery, generated clients, binaries, type declarations, peer dependency contracts, or tool configs.
- Files no longer reachable from declared entrypoints, routing, package exports, or build/test config.

## Non-Goals

- Do not treat analyzer output as sufficient when decorators, reflection, plugin loading, routes, migrations, CLIs, or templates may reference code indirectly.
- Do not treat public API, extension points, fixtures, migrations, generated artifacts, package exports, plugin surfaces, or config-only dependencies as removable without ownership or source-of-truth proof.

## Evidence Required

- Show analyzer output when available, plus targeted reference searches proving no static references.
- Check dynamic-use surfaces: routing, registration, decorators, reflection, config, templates, manifests, package exports, scripts, docs, tests, package entrypoints, and package manager binaries.
- Identify whether the target is private, public, runtime-loaded, externally consumed, package-manager referenced, config-only, dependency-only, type-only, or peer-contract-only.
- State deletion confidence and what would need human confirmation.
- Name validation that should catch an incorrect deletion, and call out when no local validation can cover external consumers.

## Tools and Signals

Useful signals include dead-code analyzers, compiler unused warnings, dependency analyzers, import graph tools, and package-manager dependency reports.

Pair tool findings with targeted reference searches. Favor small, independently safe candidates over broad deletion sets unless the reachability proof is clear.

For Python dead-code sweeps, use `references/tools/vulture.md` for tool order, dynamic-use review, and validation gates. Do not treat `vulture` output alone as deletion proof.

## False Positives

- Framework-discovered files, routes, migrations, serializers, fixtures, plugins, macros, and reflection targets.
- CLI commands, scheduled jobs, webhooks, and external integration entrypoints.
- Public exports used by downstream consumers outside the repo, even when repo-local reference searches are empty.
- Package exports, peer dependencies, optional dependencies, build-tool dependencies, type-only dependencies, CLI binary dependencies, and config-only dependencies.
- Test snapshots, generated code, vendored code, seed data, fixture data, and data files loaded by convention.

## Wave 1 Report Checklist

- Candidate target.
- Static reference evidence.
- Dynamic-use checks performed.
- Public/private/runtime/package-manager status.
- Deletion confidence and uncertainty.

## Escalate To Parent

- Skip if the repo has too much dynamic loading and no reliable evidence path.
- Mark as Lane 2 overlap if the target is an old path, deprecated path, or compatibility branch that needs canonical-path proof.
- Mark as Lane 3 overlap if the unused target is mostly stub scaffolding or low-value AI slop.
