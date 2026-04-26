---
name: python-radon
description: "Run Radon complexity and maintainability scans on Python code and report prioritized hotspots."
---

# CANONICAL FLOW

1. DETERMINE the scan scope from the user request.
2. IF no narrower scope is given, RUN `radon` repo-wide against Python code.
3. RUN `radon cc` and `radon mi` as the default pair, exclude obvious non-source paths like `.venv`, `venv`, hidden tooling dirs, build, dist, and cache dirs.
4. REVIEW findings before acting, focus first on the highest-complexity and lowest-maintainability hotspots.
5. SEPARATE genuinely risky complexity from acceptable orchestration, generated code, one-off migrations, scripts, and concentrated-but-clear framework glue.
6. PRESENT prioritized findings in chat with file or function references and brief reasoning.
7. STOP at findings and suggested next actions unless the user explicitly asks for refactoring or deeper follow-up.

## RAILS

- KEEP the skill read-only by default.
- KEEP the canonical default to `cc + mi`; do not expand into raw or Halstead metrics unless the user asks.
- TREAT generated code, migrations, scripts, and framework wiring as lower-confidence hotspots until reviewed.
- KEEP findings ordered by highest cyclomatic complexity and lowest maintainability first.
- IF a narrower user scope is explicit, honor it; otherwise default to repo-wide Python code.
