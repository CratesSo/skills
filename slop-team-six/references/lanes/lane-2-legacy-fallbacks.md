# Lane 2: Legacy, Fallback, and Deprecated Path Removal

## Primary Goal

Find old paths, compatibility branches, feature fallbacks, deprecated APIs, and migration leftovers that are candidates for hard-cutting to one canonical path after parent triage.

This lane should prefer one current path when evidence shows the old path is unsupported, unreachable, or outside the supported compatibility window. Do not infer removal safety from names like `legacy`, `fallback`, or `compat` alone.

## Good Targets

- Branches named with `legacy`, `deprecated`, `fallback`, `compat`, `old`, `v1`, `classic`, or migration-era terms, used only as search seeds until canonical-path proof exists.
- Dual implementations where one path is canonical.
- Feature flags that are provably always on or always off across repo config, test fixtures, docs, environment examples, and known deployment/config surfaces.
- Compatibility shims for removed platforms, old schemas, old APIs, or old config names after checking persisted data and supported upgrade paths.
- Deprecated package exports, CLI flags, config aliases, or feature paths where the supported removal window is closed.
- Dependencies kept only for a deprecated path, after checking package exports, scripts, plugins, generated clients, binary entrypoints, peer contracts, and tool config.

## Non-Goals

- Do not treat live migration, rollback, downgrade, disaster-recovery, or compatibility paths as removable without proof that the supported window is closed.
- Do not treat fallback behavior for real external failures, partial outages, corrupt user data, or old persisted state as removable unless a canonical recovery path exists.
- Do not treat data migrations, importers, package exports, public CLI flags, environment variables, or config aliases as removable when existing installs, user data, downstream consumers, automation, or production state may still depend on them.

## Evidence Required

- Identify the canonical path, why it is canonical, and what source of truth establishes that.
- Show usage, config, flag, version, changelog, deprecation notice, release/support-policy, docs, deployment, or persisted-state evidence that the old path is unused, unreachable, unsupported, or past its removal window.
- Trace callers and data/state compatibility, including config aliases, persisted values, migrations, imports, package exports, CLI flags, environment variables, scripts, docs, automation, and external entrypoints.
- Identify likely deletion scope and dependent cleanup for parent triage.
- Name validation that should catch an incorrect hard cut, and call out when local tests cannot cover production config or external consumers.

## Tools and Signals

Useful signals include dead-code tools, dependency tools, package-manager reports, package export checks, config searches, feature flag inventories, docs/changelog searches, and tests covering canonical behavior.

Pair search hits with runtime, config, version, changelog, release, support-window, or persisted-state proof. Favor deleting the smallest obsolete compatibility layer that leaves the canonical path clear.

## False Positives

- Active rollback, downgrade, migration, import, or disaster-recovery paths.
- Public API, package export, CLI, or plugin compatibility still promised by version policy.
- Customer data, persisted state, docs, scripts, automation, deployment config, or environment variables that may still use old values.
- Feature flags, environment variables, package-manager scripts, config aliases, or deployment config used outside the repo.

## Wave 1 Report Checklist

- Legacy/fallback path found.
- Canonical path identified.
- Proof old path is unused, unreachable, unsupported, or outside the compatibility window.
- Likely deletion scope and dependent cleanup.
- Public API/package export/CLI/config compatibility risk.
- Migration, rollback, downgrade, persisted-data, automation, or deployment-config risk.

## Escalate To Parent

- Skip if canonical path is unclear.
- Mark as Lane 1 overlap if evidence shows the legacy path is simply unreachable dead code.
- Mark as Lane 8 overlap if the fallback is mostly defensive error handling.
