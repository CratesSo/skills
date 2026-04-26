# audit-team

**Runs a staged audit flow using scoped waves of subagents.**

**Locks the audit lens, then validates after the delegated run finishes.**

## Note

*This skill is optimized for specific, pre-configured subagents:*

- `navigator`: Use a cheap and fast model with `Low` reasoning for fast read-only mapping.
- `worker`: Use the most capable coding optimized model.
- `worker_mini`: Use a faster coding model or the most capable one with lower reasoning.
- `reviewer`: Use a coding optimized model on `High` reasoning.
- `reviewer_mini`: Use a faster coding optimized model on `High` reasoning.

## How to use

1. Type `$audit-team <audit lens>`.
2. Follow the navigator-led audit, triage, implementation, and validation flow.

### Examples

- `$audit-team repo wide, correctness`
- `$audit-team back-end, performance`
- `$audit-team frontend, security`

## Version

Current version: v1.5.0

## Install

`npx skills@latest add CratesSo/skills/audit-team`
