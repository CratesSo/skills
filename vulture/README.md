# vulture

- Run a dead-code cleanup pass for Python repos.
- Use `ruff`, `vulture`, and `pyright` in order, with small local deletions and dynamic-use skepticism.

## How to use

1. Type `$vulture <repo or cleanup target>`.
2. Follow the ordered Python dead-code pass across `ruff`, `vulture`, `pyright`, and narrow test reruns.

## Install

`npx skills@latest add CratesSo/skills/vulture`
