# Explorer Deep Brief

Use the template INSIDE the fenced block below when spawning the `explorer_deep` agent:

```text
OBJECTIVE:
Map repo structure and likely cleanup hotspots for a broad code-quality sweep. Success is a concise report covering repo structure, key files, exclusions, tool clues, and shallow cleanup signals the parent can route to lanes.

CONSTRAINTS:
Map only from observable repo evidence. Don't prove issues, assign lanes, propose fixes, or do deep investigation.
NEVER spawn subagents.

REPORT:
Write `N/A` only when a field truly doesn't apply.
Don't replace required fields with prose.
If no hotspots, still fill `## Rollup` and `## No-Hotspot Areas`.
Don't include proposed fixes, or deep issue investigation.

Use this exact schema for final report:

## ROLLUP

### Scope Covered
- repo areas inspected

### Files/Areas Sampled
- [specific files, dirs, manifests, configs, or entrypoints

### Exclusions
- generated/vendor/cache/build/dist/coverage/lockfile/fixture areas to avoid

### Notable Unknowns
- important areas not inspected or ambiguous repo facts

## REPO MAP

### Entrypoints
- main app, CLI, worker, package, service, test, or build entrypoints noticed

### Major directories
- **directory** => apparent purpose

### Test/Check Surfaces
- test dirs, scripts, CI/configs, lint/type/build commands noticed

### Generated/Vendor/Build Areas
- areas parent should usually exclude]

## TOOL CLUES

### Package Managers
- npm/pnpm/yarn/pip/cargo/go/dotnet/gradle/etc.

### Repo-Native Scripts/Checks
- script or command names noticed, not install recommendations

### Analyzer/Config Files:
- tsconfig, eslint, ruff, mypy, clippy, swiftlint, etc.

## Hotspots

### H1: [short name]
- **Paths**:
  - specific files or directories
- **Raw Signal**:
  - what looks cleanup-relevant from shallow mapping
- **Suggested Next Look**:
  - specific file, command, or reference the parent should inspect next
- **Boundary/Risk**:
  - why this may be noisy, generated, dynamic, public, or otherwise risky

(Repeat as `H2`, `H3`, etc.)

## No-Hotspot Areas
- **[area checked]**: why no obvious cleanup signal surfaced
```
