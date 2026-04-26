# Lane 7: Legacy, Fallback, and Deprecated Path Removal

## Primary Goal

Find old paths, compatibility branches, feature fallbacks, deprecated APIs, and migration leftovers that can be hard-cut to one canonical path.

This lane should aggressively prefer one current path when evidence shows the old path is no longer needed.

## Good Targets

- `legacy`, `deprecated`, `fallback`, `compat`, `old`, `v1`, `classic`, or migration-era branches.
- Dual implementations where one path is canonical.
- Feature flags that are always on or always off.
- Compatibility shims for removed platforms, old schemas, old APIs, or old config names.
- Dependencies kept only for a deprecated path.

## Non-Goals

- Do not remove live migration, rollback, or compatibility paths without proof that the supported window is closed.
- Do not remove fallback behavior for real external failures unless a canonical recovery path exists.
- Do not touch data migrations that are needed for existing installs or production state.

## Evidence Required

- Identify the canonical path and why it is canonical.
- Show usage, config, flag, version, or docs evidence that the old path is dead.
- Trace callers and data/state compatibility.
- Identify deletion scope and dependent cleanup.
- Note compatibility risk the parent should consider during triage.

## Tools and Signals

Useful signals include dead-code tools, dependency tools, package-manager reports, config searches, feature flag inventories, docs searches, and tests covering canonical behavior.

Good command classes: `knip`, `vulture`, `cargo udeps`, `composer-unused`, package dependency reports, targeted search for legacy terms, and repo-native checks that prove canonical behavior.
Pair search hits with runtime/config proof.

## False Positives

- Active rollback, migration, import, or disaster-recovery paths.
- Public API compatibility still promised by version policy.
- Customer data or config that may still use old values.
- Feature flags used outside the repo.

## Evidence To Gather

- Legacy/fallback path found.
- Canonical path identified.
- Proof old path is unused or unsupported.
- Deletion scope and dependent cleanup.
- Migration/rollback risk.

## Escalate To Parent

Skip if canonical path is unclear.
