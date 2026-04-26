Read this during preflight before spawning `Wave 1`.

# How To Use This File

- Build a short `AVAILABLE TOOLS AND COMMANDS` section for each lane handoff containing only tools already available or tools the user approved installing.
- Do not pass whole list to subagents.

## Parent Tool Rules

- Detect repo languages, package managers, monorepo layout, and native check commands first.
- Prefer repo-native scripts over global commands when both exist.
- If a useful tool is absent, ask the user if they want to install it before proceeding with `Wave 1`.
- Never pass bare `npx` examples to subagents. Use repo scripts, checked local binaries, or user-approved installs.
- Exclude generated, vendor, cache, build, dist, coverage, lockfile, and fixture areas unless the lane specifically targets them.
- Commands below are examples to adapt to the repo root, package manager, and installed tool names.

## Tooling Matrix

### JavaScript and TypeScript

- Baseline checks: `npm run typecheck`, `npm run lint`, `npm test -- --runInBand`, `./node_modules/.bin/tsc --noEmit`.
- Lane 1 dedupe: `./node_modules/.bin/jscpd . --ignore "**/node_modules/**,**/dist/**,**/build/**"`, `./node_modules/.bin/eslint . --rule "no-duplicate-imports:error"`.
- Lane 2 shared types: `./node_modules/.bin/tsc --noEmit --pretty false`, `./node_modules/.bin/eslint . --ext .ts,.tsx`.
- Lane 3 unused code: `./node_modules/.bin/knip`, `./node_modules/.bin/ts-prune`, `./node_modules/.bin/tsc --noEmit --noUnusedLocals --noUnusedParameters`.
- Lane 4 cycles: `./node_modules/.bin/depcruise --no-config --output-type err .`, `./node_modules/.bin/madge --circular --extensions ts,tsx,js,jsx src`.
- Lane 5 weak types: `./node_modules/.bin/tsc --noEmit --pretty false`, `./node_modules/.bin/eslint . --rule "@typescript-eslint/no-explicit-any:error"`.
- Lane 6 defensive errors: `./node_modules/.bin/eslint . --rule "no-empty:error" --rule "no-useless-catch:error"`.
- Lane 7 legacy paths: `./node_modules/.bin/knip`, `npm ls`, targeted search for `deprecated|legacy|fallback|compat|old`.
- Lane 8 slop: repo slop scanner if present, lint warnings, targeted search for `TODO|stub|placeholder|temporary|magic|hack`.

### Python

- Baseline checks: `python -m pytest`, `python -m ruff check .`, `python -m mypy .`, `pyright`.
- Lane 1 dedupe: `python -m ruff check . --select SIM,PLR`, `python -m pylint <package> --disable=all --enable=duplicate-code`.
- Lane 2 shared types: `python -m mypy .`, `pyright`, search for duplicate `TypedDict`, `Protocol`, dataclass, and pydantic shapes.
- Lane 3 unused code: `python -m vulture .`, `python -m ruff check . --select F401,F841`.
- Lane 4 cycles: `python -m pydeps <package> --show-cycles`, `python -m grimp <package>`.
- Lane 5 weak types: `python -m mypy . --warn-unused-ignores`, `pyright`, search for `Any`, `type: ignore`, untyped defs.
- Lane 6 defensive errors: `python -m ruff check . --select E722,B,B904,TRY`, search for broad `except Exception`.
- Lane 7 legacy paths: `python -m vulture .`, targeted search for `legacy|fallback|deprecated|compat`.
- Lane 8 slop: `python -m ruff check . --select ERA,FIX`, targeted search for low-value comments and stubs.

### Rust

- Baseline checks: `cargo test`, `cargo clippy --all-targets --all-features -- -D warnings`, `cargo fmt --check`.
- Lane 1 dedupe: `cargo clippy --all-targets --all-features`, search for repeated impl blocks and match arms.
- Lane 2 shared types: `cargo clippy --all-targets --all-features`, search for duplicate structs/enums across crates.
- Lane 3 unused code: `cargo udeps --all-targets`, `cargo machete`, compiler dead-code warnings.
- Lane 4 cycles: `cargo tree -d`, `cargo tree --edges normal,build,dev`.
- Lane 5 weak types: clippy casts, `Box<dyn Any>`, stringly typed enums, overly broad generics.
- Lane 6 defensive errors: clippy `needless_match`, `map_err`, broad catch-all error mapping.
- Lane 7 legacy paths: `cargo udeps`, feature flag review, search for `legacy|fallback|deprecated|compat`.
- Lane 8 slop: clippy pedantic signals where repo already uses them, search for placeholder comments.

### Go

- Baseline checks: `go test ./...`, `go vet ./...`, `gofmt -d .`, `staticcheck ./...`.
- Lane 1 dedupe: `gofmt -d .`, `staticcheck ./...`, search for repeated helpers and copy-pasted table tests.
- Lane 2 shared types: `go vet ./...`, search for duplicate structs/interfaces in sibling packages.
- Lane 3 unused code: `go test ./...`, `staticcheck ./...`, `go list -deps ./...`.
- Lane 4 cycles: `go list -deps -json ./...`, import graph review; Go compiler catches direct cycles.
- Lane 5 weak types: search for `interface{}`, `any`, `map[string]interface{}`, unchecked type assertions.
- Lane 6 defensive errors: `staticcheck ./...`, search for ignored errors and redundant `if err != nil` wrappers.
- Lane 7 legacy paths: `go mod why -m <module>`, search for deprecated packages and compat branches.
- Lane 8 slop: `staticcheck ./...`, search for TODOs, panics in non-test code, placeholder comments.

### Swift, C, C++, and Shell

- Swift baseline: `swift build`, `swift test`, `swiftlint lint`, `periphery scan`.
- Swift lane 3: `periphery scan`, compiler unused warnings.
- Swift lane 5: `swiftlint lint`, compiler diagnostics, search for `Any`, force casts, force unwraps.
- C/C++ baseline: build command, `clang-tidy`, `include-what-you-use`, compiler warnings.
- C/C++ lane 3: linker/compiler unused warnings, `include-what-you-use`.
- C/C++ lane 4: include graph review, `include-what-you-use`, build-system dependency graph.
- Shell baseline: `shellcheck scripts/*.sh`, `shfmt -d scripts`.
- Shell lanes 6/8: `shellcheck`, search for swallowed errors, dead flags, placeholder comments.

### JVM, .NET, PHP, and Ruby

- JVM baseline: `./gradlew test`, `./gradlew check`, `mvn test`, `jdeps`, `spotbugs`, `detekt`.
- .NET baseline: `dotnet test`, `dotnet build`, `dotnet format --verify-no-changes`, Roslyn analyzers.
- PHP baseline: `composer test`, `vendor/bin/phpstan analyse`, `vendor/bin/psalm`, `composer-unused`.
- Ruby baseline: `bundle exec rspec`, `bundle exec rubocop`, `bundle exec steep check`, `debride`.
- Use lane mapping by category: duplicate detectors for Lane 1, dependency graph tools for Lane 4, type analyzers for Lane 5, unused/dependency analyzers for Lane 3 and Lane 7, lint/search for Lane 6 and Lane 8.

## Parent Handoff Guidance

If no useful lane-specific tool is available, say that directly and provide targeted search commands or repo-native checks instead.

For each active lane, include a compact tool block in the `Wave 1` handoff using the exact `AVAILABLE TOOLS AND COMMANDS` schema from `references/wave-1.md`. Keep each block lane-specific and omit tools the parent did not verify as available or user-approved.
