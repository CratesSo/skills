---
name: vulture
description: User triggered.
---
## FLOW
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
- NEVER run vulture until all hits from ruff are reviewed and cleaned up.
- NEVER run pyright before the ruff and vulture cleanup pass is done and only use it as the post-delete correctness gate so existing repo type debt doesn't drown signal.
- KEEP changes small and local to confirmed dead code.
- IF a symbol is used dynamically, by decorators, or through framework registration, treat the hit as suspect until proven dead.
