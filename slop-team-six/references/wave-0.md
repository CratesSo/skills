When spawning the repo mapping agent, provide the full absolute path to `${SKILL_ROOT}/references/roles/explorer-agents.md` and use the entire template below as their spawn prompt:

# EXPLORER OBJECTIVES

1. Read and follow `[insert full path to explorer-agents.md here]` to map repo structure and likely cleanup hotspots for a broad code-quality sweep. Success is a concise report covering repo structure, key files, exclusions, tool clues, and concrete cleanup signals.
2. Report findings using `EXPLORER OUTPUT TEMPLATE`.

## EXPLORER CONSTRAINTS

- Never engage in speculating or investigating potential/specific issues yourself. Simply explore, map, and report.
- NEVER spawn subagents!

## EXPLORER OUTPUT TEMPLATE

- Write `N/A` only when a field truly does not apply.
- If there are no hotspots, still fill `## Rollup` and include `## No-Hotspot Areas`.

Report your final results using the template below:

## Rollup

- **Scope covered:** [repo areas inspected]
- **Files/areas sampled:** [specific files, dirs, manifests, configs, or entrypoints]
- **Exclusions:** [generated/vendor/cache/build/dist/coverage/lockfile/fixture areas to avoid]
- **Notable unknowns:** [important areas not inspected or ambiguous repo facts]

## Repo Map

- **Entrypoints:** [main app, CLI, worker, package, service, test, or build entrypoints noticed]
- **Major directories:** [directory => apparent purpose]
- **Test/check surfaces:** [test dirs, scripts, CI/configs, lint/type/build commands noticed]
- **Generated/vendor/build areas:** [areas parent should usually exclude]

## Tool Clues

- **Package managers:** [npm/pnpm/yarn/pip/cargo/go/dotnet/gradle/etc.]
- **Repo-native scripts/checks:** [script or command names noticed, not install recommendations]
- **Analyzer/config files:** [tsconfig, eslint, ruff, mypy, clippy, swiftlint, etc.]

## Hotspots

### H1: [short name]

- **Paths:** [specific files or directories]
- **Raw signal:** [what looks cleanup-relevant from shallow mapping]
- **Boundary/risk:** [why this may be noisy, generated, dynamic, public, or otherwise risky]

Repeat as `H2`, `H3`, etc. for additional hotspots.

### No-Hotspot Areas

- [area checked]: [why no obvious cleanup signal surfaced]
