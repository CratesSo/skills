---
name: bandit
description: "Run Bandit security scans on Python code and report prioritized findings."
---

# CANONICAL FLOW

1. DETERMINE scan scope from user request.
2. IF no narrower scope is given, RUN the bundled runner repo-wide against Python code:
   `python3 /Users/admin/.codex/skills/bandit/scripts/run_bandit.py <scope>`
3. The runner MUST do recursive scanning, exclude obvious non-source paths like `.venv`, `venv`, `__pycache__`, `.git`, `.tox`, build, dist, and cache dirs, and use moderate signal defaults: medium+ severity and medium+ confidence.
4. REVIEW hits before acting, separate real security issues from framework noise, test-only code, fixtures, and intentional patterns.
5. PRESENT prioritized findings with file references and brief reasoning.
6. STOP at findings and suggested next actions unless user explicitly asks for remediation or follow-up.

## RAILS

- STAY read-only by default.
- NEVER make code changes just because `bandit` emitted a hit.
- TREAT test-only code, fixtures, and framework-specific patterns as suspect until reviewed, not auto-valid.
- ORDER findings by severity.
