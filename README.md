# Skills

*Each skill is ranked from most to least powerful inside its category.*

## Codex Specific

#### Plan My Grill

!! WARNING: This is a good skill !!
Interrogate plans and designs until they are handoff-ready.

```sh
npx skills@latest add CratesSo/skills/plan-my-grill
```

#### Actions

Manage workspace actions in `.codex/environments/environment.toml`.

```sh
npx skills@latest add CratesSo/skills/actions
```

## Code Review

#### Fix

Investigate technical failures before applying narrow, verified fixes.

```sh
npx skills@latest add CratesSo/skills/fix
```

#### Audit Team

Coordinate agentic audit workflows from scope mapping through triage and fixes.

```sh
npx skills@latest add CratesSo/skills/audit-team
```

#### Slop Team Six

Run evidence-backed cleanup sweeps using subagents and lane playbooks.

```sh
npx skills@latest add CratesSo/skills/slop-team-six
```

#### Python Radon

Run Radon complexity and maintainability scans on Python code.

```sh
npx skills@latest add CratesSo/skills/python-radon
```

#### Preflight

Run production-readiness preflight checks across security, database, deployment, code, and Bandit scanning for Python.

```sh
npx skills@latest add CratesSo/skills/preflight
```

#### Vulture

Find and remove confirmed Python dead code.

```sh
npx skills@latest add CratesSo/skills/vulture
```

## Quality of Life

#### Agents Doctor

Audit repo AGENTS.md files for safe cleanup opportunities.

```sh
npx skills@latest add CratesSo/skills/agents-doctor
```

#### Agents Splitter

Split large AGENTS.md guidance into on-demand reference files.

```sh
npx skills@latest add CratesSo/skills/agents-splitter
```

#### Lessons Doctor

Move durable repo lessons into local AGENTS.md guidance.

```sh
npx skills@latest add CratesSo/skills/lessons-doctor
```

#### Report

Generate complete standalone HTML reports from recap or custom requests.

```sh
npx skills@latest add CratesSo/skills/report
```

#### Handoff

Generate concise continuation prompts from current thread context and tool results.

```sh
npx skills@latest add CratesSo/skills/handoff
```

#### Todo

Manage a repo-root `todo.md` with durable four-character item references.

```sh
npx skills@latest add CratesSo/skills/todo
```

#### YOLO

Finish explicit requests without tests, cleanup, or validation gates.

```sh
npx skills@latest add CratesSo/skills/yolo
```

## Versions

| Skill | Directory | Current version | Tag |
| --- | --- | --- | --- |
| **actions** | `actions/` | v1.0.6 | `actions/v1.0.6` |
| **agents-doctor** | `agents-doctor/` | v1.0.5 | `agents-doctor/v1.0.5` |
| **agents-splitter** | `agents-splitter/` | v1.0.3 | `agents-splitter/v1.0.3` |
| **audit-team** | `audit-team/` | v1.5.2 | `audit-team/v1.5.2` |
| **fix** | `fix/` | v1.0.0 | `fix/v1.0.0` |
| **handoff** | `handoff/` | v1.0.3 | `handoff/v1.0.3` |
| **lessons-doctor** | `lessons-doctor/` | v1.0.4 | `lessons-doctor/v1.0.4` |
| **plan-my-grill** | `plan-my-grill/` | v1.6.2 | `plan-my-grill/v1.6.2` |
| **preflight** | `preflight/` | v1.0.2 | `preflight/v1.0.2` |
| **python-radon** | `python-radon/` | v1.0.0 | `python-radon/v1.0.0` |
| **report** | `report/` | v1.1.0 | `report/v1.1.0` |
| **slop-team-six** | `slop-team-six/` | v2.0.3 | `slop-team-six/v2.0.3` |
| **todo** | `todo/` | v0.1.1 | `todo/v0.1.1` |
| **vulture** | `vulture/` | v1.0.0 | `vulture/v1.0.0` |
| **yolo** | `yolo/` | v1.0.0 | `yolo/v1.0.0` |

## Install / Pin

Install a skill from this repo with `npx skills@latest add`.

Examples:

- `npx skills@latest add CratesSo/skills/actions`
- `npx skills@latest add CratesSo/skills/plan-my-grill`

Use the matching tag when pinning a published version.

Examples:

- `CratesSo/skills@actions/v1.0.5`
- `CratesSo/skills@plan-my-grill/v1.6.0`
