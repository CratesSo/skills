---
name: slop-team-six
description: User triggered.
---
Run execution-first cleanup sweep. Scan fast, activate the right cleanup lanes, prove relevance before edits, execute in two waves, review risky work, validate narrowly, and report by lane. Follow instructions below carefully and exactly.

# DEFAULTS
- Default scope is whole repo.
- Default stance is aggressive cleanup with evidence.
- Use repo-native tools when present. If a tool is absent, install it.
- Always spawn agents with `fork_context: false`.

# CANONICAL FLOW
1. Spawn `navigator` agent to map repo to the eight cleanup lanes.
2. While `navigator` works, run `preflight` check.
3. When `navigator` is done, decide which lanes are active, skipped, or merged based on `navigator` output.
4. Spawn `Wave 1` read-only `cleanup` agents for evidence gathering: Instruct them what tools to use and what to look for based on `navigator` output.
5. When all `cleanup` agents have finished, triage `Wave 1` results.
6. When `Wave 1` results are triaged, spawn `Wave 2` worker agents only for validated implementation.
7. Run `reviewer` on higher-risk lanes after work is complete.
8. Run narrowest useful validation when fully complete.
9. Return track-by-track summary with risks and leftovers.

## PREFLIGHT
- `preflight` means finding:
  - languages in use
  - workspace layout
  - type systems and compiler configs
  - available analysis tools by language, or repo equivalents:
    - JS/TS: `knip`, `dependency-cruiser` or `madge`, `tsc`, `eslint`, `ts-prune`
    - Python: `pyright`, `mypy`, `ruff`, `vulture`; Rust: `cargo clippy`, `cargo udeps`; Go: `staticcheck`, `go vet`, `golangci-lint`
    - JVM: `jdeps`, `errorprone`, `spotbugs`, `detekt`; .NET: `dotnet build`, `dotnet format`, `roslynator`
    - PHP/Ruby: `phpstan`, `psalm`, `composer-unused`, `deptrac`, `rubocop`, `sorbet` or `steep`, `debride`
    - Swift/C/C++/Shell: `swift build`, `swiftlint`, `periphery`, `clang-tidy`, `include-what-you-use`, `shellcheck`, `shfmt`
  - IF A TOOL THAT WOULD BE USEFUL IS ABSENT, STOP AND GET PERMISSION TO INSTALL IT BEFORE PROCEEDING WITH THE AUDIT.
  - repo-native test, lint, or check commands
  - obvious generated, vendor, cache, dist, build, coverage, and lockfile areas to exclude

### Navigator Brief
- Use this exact shape when spawning `navigator`:

```text
OBJECTIVE:
- Map repo structure and likely cleanup hotspots for a broad code-quality sweep. Success is a concise report covering the repo structure and key files.

CONSTRAINTS:
- Never engage in speculating or investigating potential/specific issues yourself. Simply explore, map, and report. Never use subagents!
```

## EIGHT LANES
- These are the cleanup lanes. Keep in this order:
  1. Dedupe and consolidation
  2. Shared type consolidation
  3. Unused code discovery and removal
  4. Circular dependency removal
  5. Weak type replacement
  6. Unnecessary defensive error handling removal
  7. Legacy, fallback, and deprecated path removal
  8. AI slop, stubs, and low-value comments cleanup

- Don't force all eight lanes to edit (execution is adaptive).
- Each lane must end in one of three states before spawning Wave 2:
  - `ACTIVE`: enough evidence to edit
  - `SKIP`: repo does not meaningfully support lane
  - `MERGE`: lane overlaps another lane enough that one worker should own both

## WAVE 1: EVIDENCE
- For each active lane, spawn a read-only `cleanup` agent to gather evidence and propose edits. Instruct them to use repo-native tools where possible and to produce a cleanup brief with findings and risk notes.

- Tool-backed lanes should use native tools when available:
  - Lane 3: prefer dead-code analyzers such as `knip`, `ts-prune`, `vulture`, `cargo udeps`, `periphery`, `debride`, and compiler unused warnings
  - Lane 4: prefer dependency analyzers such as `dependency-cruiser`, `madge`, `jdeps`, `deptrac`, or native package graph tools
  - Lane 5: prefer strongest type and compiler analyzers such as `tsc`, `pyright`, `mypy`, `cargo clippy`, `staticcheck`, `phpstan`, `psalm`, `detekt`, `clang-tidy`, or Roslyn analyzers
  - Other lanes: lint rules, static inspection, targeted search, and language analyzers as available
  - If a tool is absent and needed to perform useful analysis, stop and get permission from user to install it and continue with the audit once installation is complete.

### Cleanup Brief
- Use this exact shape when spawning `cleanup` agents for Wave 1:

```text
GOAL:
- [describe their lane, task, success condition, and tools to use]
- Never use subagents!
- ALWAYS stay in read-only mode and never implement changes! ONLY report findings, evidence, and risk notes in a cleanup brief.

RELEVANT FILES:
- [files and configs tied to lane scope]

AVOID:
- [any overlapping files owned by parallel agents]
```

## TRIAGE
You own triage. Never pass raw Wave 1 output straight into edits.

- Before Wave 2:
  - Merge duplicate findings.
  - Reject weak or overlapping proposals.
  - Collapse related lanes when they share the same files or root cause.
  - Decide which lanes need `worker_mini` versus `worker`.

## WAVE 2: EXECUTION
- Wave 2 implements only validated cleanup work.
  - Use `worker_mini` for narrow, obvious cleanup slices.
  - Use `worker` for cross-file or high risk cleanup slices.
- Keep write scopes non-overlapping when parallel workers run.

### Worker Brief
- Use this exact shape when spawning worker agents for Wave 2:

```text
GOAL:
- [describe task and success condition]
- Never use subagents!

RELEVANT FILES:
- [files tied to scope]

AVOID:
- [any overlapping files owned by parallel worker agents]
```

### Lane Rules 
Pass on specific lane rules to cleanup agents as constraints in their brief. Here are some examples, but adapt based on repo context and `navigator` findings:
- Lane 1: dedupe only when it reduces complexity or deletes branching. Don't extract abstractions that add more code than they remove.
- Lane 2: consolidate shared types only when ownership becomes clearer and call sites simplify.
- Lane 3: treat framework registration, reflection, decorators, plugin systems, and dynamic loading as suspect until proven unused.
- Lane 4: break cycles with the smallest structural change that keeps dependency flow boring and explicit.
- Lane 5: replace `any`, `unknown`, and equivalent weak types only after tracing usage sites and checking upstream or adjacent source types.
- Lane 6: remove `try/catch` or equivalent defensive code only when it hides errors, swallows failures, or guards impossible scenarios. Preserve boundaries that sanitize input, convert external errors, or perform intentional recovery.
- Lane 7: remove fallback, legacy, deprecated, and parallel code paths aggressively once canonical path is clear.
- Lane 8: bias toward deleting comments, stubs, placeholders, and AI slop. Keep or add comments only when they help a new engineer understand stable behavior.

## VALIDATION
- Run narrowest useful validation for changed scope.
- Prefer lane-relevant checks first.
- Use repo-native tests, compilers, linters, and analyzers.

## FINAL RESPONSE
- Keep final response concise and return results grouped by lane, even if multiple lanes merged during execution, using exact template below:

```md
## PREFLIGHT
- Languages: [list languages]
- Tools Used: [list tools]
- Exclusions: [list exclusions]
## LANE STATUS
- **Lane 1:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 2:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 3:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 4:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 5:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 6:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 7:** `ACTIVE` | `SKIP` | `MERGE`
- **Lane 8:** `ACTIVE` | `SKIP` | `MERGE`
## CHANGES BY LANE
### Lane n:
- **EVIDENCE:** [brief summary]
- **EDITS:** [brief summary]
- **VALIDATION:** [brief summary]
- **RISKS:** [brief summary]
## RESIDUAL
- [blockers, deferred lanes, or remaining risks]
```
