# Lane 3: Unused Code Discovery and Removal

## Primary Goal

Find code, exports, dependencies, files, or configuration that appear unused and can likely be deleted.

This lane requires stronger proof than most lanes because dynamic registration and framework conventions create false positives.

## Good Targets

- Unreferenced functions, classes, components, constants, files, exports, or package dependencies.
- Dead test helpers, scripts, feature modules, old entrypoints, and stale config.
- Unused imports, variables, parameters, and private methods.
- Dependencies present in manifests but absent from imports, runtime loading, or scripts.
- Files no longer reachable from entrypoints.

## Non-Goals

- Do not treat analyzer output as sufficient when decorators, reflection, plugin loading, routes, migrations, CLIs, or templates may reference code indirectly.
- Do not propose deleting public API, extension points, fixtures, or generated artifacts without ownership proof.

## Evidence Required

- Show analyzer output or targeted search proving no static references.
- Check dynamic-use surfaces: routing, registration, decorators, reflection, config, templates, scripts, docs, and tests.
- Identify whether the symbol/file is public or private.
- State deletion confidence and what would need human confirmation.
- Name behavior or caller risk if the deletion is wrong.

## Tools and Signals

Useful signals include dead-code analyzers, compiler unused warnings, dependency analyzers, import graph tools, and package-manager dependency reports.

Good command classes: `knip`, `ts-prune`, `vulture`, `ruff F401/F841`, `cargo udeps`, `cargo machete`, `periphery`, `staticcheck`, `composer-unused`, `debride`, compiler warnings.
Pair tool findings with targeted reference searches.

## False Positives

- Framework-discovered files, routes, migrations, serializers, fixtures, plugins, macros, and reflection targets.
- CLI commands, scheduled jobs, webhooks, and external integration entrypoints.
- Public exports used by downstream consumers outside the repo.
- Test snapshots, generated code, vendored code, and data files.

## Evidence To Gather

- Candidate deletion target.
- Static reference evidence.
- Dynamic-use surfaces checked.
- Public/private status.
- Deletion confidence and risk.

## Escalate To Parent

Skip if the repo has too much dynamic loading and no reliable evidence path.
