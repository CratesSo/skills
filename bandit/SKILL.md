---
name: bandit
description: User triggered.
---
## FLOW
1. DETERMINE the scan scope from user request.
2. IF no narrower scope is given, RUN `bandit` repo-wide against Python code.
3. RUN with recursive scanning, exclude obvious non-source paths like `.venv`, `venv`, `__pycache__`, `.git`, `.tox`, build, dist, and cache dirs, and use moderate signal defaults: medium+ severity and medium+ confidence.
4. REVIEW hits before acting, separate real security issues from framework noise, test-only code, fixtures, and intentional patterns.
5. PRESENT prioritized findings in chat with file references and brief reasoning.
6. STOP at findings and suggested next actions unless the user explicitly asks for remediation or deeper follow-up.

## RAILS
- KEEP the skill read-only by default.
- NEVER make code changes just because `bandit` emitted a hit.
- TREAT `# nosec`, test-only code, fixtures, and framework-specific patterns as suspect until reviewed, not auto-valid.
- KEEP findings ordered by severity and confidence first, then by likely impact.
