# Radon Tool Reference

Use this as the Python complexity and maintainability tool reference for Lane 7, with optional Lane 4 prioritization. Only pass this reference when Python files are in scope.

## Purpose

Run Radon complexity and maintainability scans on Python code and report prioritized hotspots.

Radon output is a prioritization signal only. It does not authorize edits by itself.

## Scope

- Honor an explicit narrower user scope.
- If no narrower scope is given, scan repo-wide Python code.
- Exclude obvious non-source paths: `.venv`, `venv`, hidden tooling dirs, generated files, build, dist, cache, coverage, vendored code, and fixtures unless explicitly in scope.

## Default Commands

Run cyclomatic complexity and maintainability as the default pair:

```bash
radon cc <scope>
radon mi <scope>
```

Do not expand into raw metrics or Halstead metrics unless the user asks.

## Review Rules

- Focus first on highest-complexity and lowest-maintainability hotspots.
- Separate genuinely risky complexity from acceptable orchestration, generated code, one-off migrations, scripts, and concentrated-but-clear framework glue.
- For Lane 7, look for weak, broad, or ambiguous types that create branching, casts, shape checks, or defensive code.
- For Lane 4, use Radon only to prioritize complex repeated branches that still need duplication evidence from other commands or manual inspection.

## Evidence To Capture

For each hotspot, capture:

- file and function/class reference
- Radon signal: complexity rank or maintainability concern
- why it matters for the active lane
- whether the hotspot is likely acceptable framework glue, orchestration, generated code, or migration code
- validation expectation or parent triage note
