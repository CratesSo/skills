# slop-team-six

**Runs an execution-first cleanup sweep using scoped subagents.**

**Scans, activates relevant cleanup lanes, proves relevance before edits, executes in two waves, reviews risky work, validates narrowly, and reports by lane.**

## Note

*This skill is optimized for specific, pre-configured subagents:*

- `navigator`: Use a cheap and fast model with low reasoning for fast read-only mapping.
- `cleanup`: Use any solid model in read-only mode.
- `worker`: Use the most capable coding optimized model.
- `worker_mini`: Use a faster coding model or the most capable one with lower reasoning.

## How to use

1. Type `$slop-team-six <repo or cleanup lens>`.
2. Follow the staged cleanup sweep across preflight, evidence, triage, execution, review, and narrow validation.
3. If tools are needed to perform audit, you will be prompted to install them before proceeding.

## Version

Current version: v2.0.1

## Install

`npx skills@latest add CratesSo/skills/slop-team-six`
