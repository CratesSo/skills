# Vulture Tool Reference

Use this as the Python dead-code tool reference for Lane 1, and as an optional Lane 2 signal for dead legacy paths. Only pass this reference when Python files are in scope.

## Purpose

Find and remove confirmed Python dead code using `ruff`, `vulture`, `pyright`, and targeted tests.

Tool output is evidence only. Deletion requires parent-triaged proof that dynamic and framework-driven use is not present.

## Tool Check

Verify tools are installed and runnable before using this flow. Install only when the parent handoff explicitly approves these commands:

```bash
command -v ruff || uv tool install ruff
command -v vulture || uv tool install vulture
command -v pyright || uv tool install pyright
ruff --version && vulture --version && pyright --version
```

Use globally installed `uv tool` binaries when available. Do not install tool dependencies into the target repo just to run cleanup.

If a tool install succeeds but execution is killed, stop and diagnose the local security block before continuing.

## Command Order

For `Wave 1` evidence gathering:

1. Run `ruff check` on Python files in scope.
2. Review hits and separate real dead code from dynamic/framework-driven code.
3. Run `vulture` on Python files in scope only after `ruff` hits are reviewed.
4. Review `vulture` hits and separate real dead code from dynamic/framework-driven code.
5. Report confirmed candidates for parent triage; do not delete files or code in `Wave 1`.

For `Wave 2` implementation, delete only parent-approved dead code from a validated implementation brief, then run `pyright`, `ruff`, and relevant tests on the touched scope.

Do not run `pyright` before the `ruff` and `vulture` evidence pass is complete; use it as the post-delete correctness gate so existing repo type debt does not drown signal.

## Scope Rules

- Exclude `.venv`, `venv`, generated, vendor, cache, build, dist, coverage, and fixture areas unless explicitly in scope.
- Keep changes small and local to confirmed dead code.
- If a symbol is used dynamically, by decorators, or through framework registration, treat the hit as suspect until proven dead.

## Evidence Required

For each candidate, report:

- analyzer output and location
- static reference search result
- dynamic-use checks performed
- public/private status
- deletion confidence
- validation expectation that should fail if deletion is wrong
