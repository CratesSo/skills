# Tooling Matrix

Read this during preflight before spawning Wave 1.

Do not pass this whole file to subagents. Build short `PARENT-APPROVED INSTALL COMMANDS` and `AVAILABLE TOOLS AND COMMANDS` sections for each lane handoff.

## Parent Tool Rules

- Detect repo languages, package managers, monorepo layout, and native check commands first.
- Prefer repo-native scripts over global commands when both exist.
- If a useful analysis tool is absent, install shared tools before Wave 1 or add an explicit parent-approved lane-scoped install command to the Wave 1 handoff.
- Prefer global tool managers and temporary caches over adding dependencies to the target repo. Do not modify repo manifests or lockfiles just to run analysis.
- Hooky slop scanner helpers (`semgrep`, `ast-grep`, `shellcheck`, `shfmt`) may be installed when useful; otherwise report missing helpers as coverage gaps.
- Wave 1 agents may install missing analysis tools only when the parent handoff explicitly includes the install command; they must report installed tools in their rollup.
- Commands below are examples to adapt to the repo root, package manager, and installed tool names.
- Prefer repo scripts and local binaries. Use `npx` only when intentionally using a temporary analysis tool.
- When converting examples to `npm exec`, keep tool flags before positional paths, for example `npm exec -- madge --circular src`. Do not insert an extra `--` before tool flags.
- Exclude generated, vendor, cache, build, dist, coverage, lockfile, and fixture areas unless the lane specifically targets them.

## JavaScript and TypeScript

- Baseline checks: `npm run typecheck`, `npm run lint`, `npm test -- --runInBand`, `npx tsc --noEmit`.
- Lane 1 unused code: `npx knip`, `npx tsc --noEmit --noUnusedLocals --noUnusedParameters`. Do not use Knip `--exclude` for path globs; it excludes Knip issue types, so use config or triage path noise after the run.
- Lane 2 legacy paths: `npx knip`, `npm ls`, targeted search for `deprecated|legacy|fallback|compat|old`.
- Lane 3 slop: hooky slop scanner when available using `references/tools/slop-scan.md`, repo slop scanner if present, lint warnings, targeted search for `TODO|stub|temporary|magic|hack`.
- Lane 4 dedupe: `npx jscpd . --ignore "**/node_modules/**,**/dist/**,**/build/**"`, `npx eslint . --rule "no-duplicate-imports:error"`.
- Lane 5 shared types: `npx tsc --noEmit --pretty false`, `npx eslint . --ext .ts,.tsx`.
- Lane 6 cycles: `npx dependency-cruiser --no-config --output-type err .`, `npx madge --circular --extensions ts,tsx,js,jsx src`.
- Lane 7 weak types: `npx tsc --noEmit --pretty false`, `npx eslint . --rule "@typescript-eslint/no-explicit-any:error"`.
- Lane 8 defensive errors: `npx eslint . --rule "no-empty:error" --rule "no-useless-catch:error"`.

## Python

- Baseline checks: `python -m pytest`, `python -m ruff check .`, `python -m mypy .`, `pyright`.
- Lane 1 unused code: when Python files are in scope, Python dead-code flow using `references/tools/vulture.md`, `python -m vulture .`, `python -m ruff check . --select F401,F841`.
- Lane 2 legacy paths: when Python files are in scope, optional `references/tools/vulture.md`, `python -m vulture .`, targeted search for `legacy|fallback|deprecated|compat`.
- Lane 3 slop: `python -m ruff check . --select ERA,FIX`, targeted search for low-value comments and stubs.
- Lane 4 dedupe: when Python files are in scope, `python -m ruff check . --select SIM,PLR`, optional Radon prioritization using `references/tools/radon.md`, targeted duplicate search.
- Lane 5 shared types: `python -m mypy .`, `pyright`, search for duplicate `TypedDict`, `Protocol`, dataclass, and pydantic shapes.
- Lane 6 cycles: `python -m pydeps <package> --show-cycles`, `python -m grimp <package>`.
- Lane 7 weak types: when Python files are in scope, `python -m mypy . --warn-unused-ignores`, `pyright`, Radon complexity flow using `references/tools/radon.md`, search for `Any`, `type: ignore`, untyped defs.
- Lane 8 defensive errors: `python -m ruff check . --select E722,B,B904,TRY`, search for broad `except Exception`.

## Rust

- Baseline checks: `cargo test`, `cargo clippy --all-targets --all-features -- -D warnings`, `cargo fmt --check`.
- Lane 1 unused code: `cargo machete`, compiler dead-code warnings, `cargo clippy --all-targets --all-features`.
- Lane 2 legacy paths: feature flag review, search for `legacy|fallback|deprecated|compat`, `cargo machete` for dependencies kept only by old paths.
- Lane 3 slop: clippy pedantic signals where repo already uses them, search for stub comments.
- Lane 4 dedupe: `cargo clippy --all-targets --all-features`, search for repeated impl blocks and match arms.
- Lane 5 shared types: `cargo clippy --all-targets --all-features`, search for duplicate structs/enums across crates.
- Lane 6 cycles: `cargo tree -d`, `cargo tree --edges normal,build,dev`.
- Lane 7 weak types: clippy casts, `Box<dyn Any>`, stringly typed enums, overly broad generics.
- Lane 8 defensive errors: clippy `needless_match`, `map_err`, broad catch-all error mapping.

## Go

- Baseline checks: `go test ./...`, `go vet ./...`, `gofmt -w` only when implementing, `staticcheck ./...`.
- Lane 1 unused code: `go test ./...`, `staticcheck ./...`, targeted reference searches.
- Lane 2 legacy paths: `go mod why -m <module>`, search for deprecated packages and compat branches.
- Lane 3 slop: `staticcheck ./...`, search for TODOs, panics in non-test code, stub comments.
- Lane 4 dedupe: `gofmt -d .`, `staticcheck ./...`, search for repeated helpers and copy-pasted table tests.
- Lane 5 shared types: `go test ./...`, `staticcheck ./...`, targeted search for duplicate structs/interfaces in sibling packages.
- Lane 6 cycles: `go list -deps -json ./...`, import graph review; Go compiler catches direct cycles.
- Lane 7 weak types: search for `interface{}`, `any`, `map[string]interface{}`, unchecked type assertions.
- Lane 8 defensive errors: `staticcheck ./...`, search for ignored errors and redundant `if err != nil` wrappers.

## Swift, C, C++, and Shell

- Swift baseline: `swift build`, `swift test`, `swiftlint lint`, `periphery scan`.
- Swift lane 1: `periphery scan`, compiler unused warnings.
- Swift lane 7: `swiftlint lint`, compiler diagnostics, search for `Any`, force casts, force unwraps.
- C/C++ baseline: build command, `clang-tidy`, `include-what-you-use`, compiler warnings.
- C/C++ lane 1: linker/compiler unused warnings, `include-what-you-use`.
- C/C++ lane 6: include graph review, `include-what-you-use`, build-system dependency graph.
- Shell baseline: `shellcheck scripts/*.sh`, `shfmt -d scripts`.
- Shell lanes 3/8: `shellcheck`, search for swallowed errors, dead flags, stub comments.

## JVM, .NET, PHP, and Ruby

- JVM baseline: `./gradlew test`, `./gradlew check`, `mvn test`, `jdeps`, `spotbugs`, `detekt`.
- .NET baseline: `dotnet test`, `dotnet build`, `dotnet format --verify-no-changes`, Roslyn analyzers.
- PHP baseline: `composer test`, `vendor/bin/phpstan analyse`, `vendor/bin/psalm`, `composer-unused`.
- Ruby baseline: `bundle exec rspec`, `bundle exec rubocop`, `bundle exec steep check`, `debride`.
- Use lane mapping by category: unused/dependency analyzers for Lanes 1 and 2, lint/search for Lane 3 and Lane 8, duplicate detectors for Lane 4, type analyzers for Lane 7, and dependency graph tools for Lane 6.

## Handoff Output

For each active lane, pass a compact tool block relevant to that lane using the `AVAILABLE TOOLS AND COMMANDS` schema in `references/wave1-cleanup.md`.

If no useful lane-specific tool is available, say that directly and provide targeted search commands or repo-native checks instead.
