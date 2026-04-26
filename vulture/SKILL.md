---
name: vulture
description: "Find and remove confirmed Python dead code using ruff, vulture, pyright, and targeted tests."
---
# CANONICAL FLOW

0. VERIFY tools are installed and runnable:
   - `command -v ruff || uv tool install ruff`
   - `command -v vulture || uv tool install vulture`
   - `command -v pyright || uv tool install pyright`
   - `ruff --version && vulture --version && pyright --version`
1. RUN `ruff check` on all python files in repo.
2. REVIEW hits, separate real dead code from dynamic/framework-driven code.
3. DELETE confirmed dead code after review.
4. RUN `vulture` on all python files in repo.
5. REVIEW hits, separate real dead code from dynamic/framework-driven code.
6. DELETE confirmed dead code after review.
7. RUN `pyright` on the touched scope to catch cleanup regressions.
8. RERUN `ruff` and the relevant tests on the touched scope.

## RAILS

- RUN with .venv excluded so only repo code counts!
- USE globally installed `uv tool` binaries when available; do not install tool dependencies into the target repo just to run cleanup.
- IF a tool install succeeds but execution is killed, stop and diagnose the local security block before continuing.
- NEVER run vulture until all hits from ruff are reviewed and cleaned up.
- NEVER run pyright before the ruff and vulture cleanup pass is done and only use it as the post-delete correctness gate so existing repo type debt doesn't drown signal.
- KEEP changes small and local to confirmed dead code.
- IF a symbol is used dynamically, by decorators, or through framework registration, treat the hit as suspect until proven dead.
