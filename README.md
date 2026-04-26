# Skills

Each skill is ranked from most to least powerful inside its category.

## Codex Specific

### 1. Plan My Grill

**WARNING: This is a good skill.**
Interrogate plans and designs until they are handoff-ready.

### 2. Actions

Manage workspace actions in `.codex/environments/environment.toml`.

## Code Review

### 1. Audit Team

Coordinate agentic audit workflows from scope mapping through triage and fixes.

### 2. Slop Team Six

Run evidence-backed cleanup sweeps using subagents and lane playbooks.

### 3. Python Radon

Run Radon complexity and maintainability scans on Python code.

### 4. Preflight

Run production-readiness preflight checks across security, database, deployment, and code.

### 5. Vulture

Find and remove confirmed Python dead code.

### 6. Bandit

Run Bandit security scans on Python code and report prioritized findings.

## Quality of Life

### 1. Report

Generate complete standalone HTML reports from recap or custom requests.

### 2. Handoff

Generate concise continuation prompts from current thread context and tool results.

### 3. YOLO

Finish explicit requests without tests, cleanup, or validation gates.

## Versions

| Skill | Directory | Current version | Tag |
| --- | --- | --- | --- |
| **actions** | `actions/` | v1.0.5 | `actions/v1.0.5` |
| **audit-team** | `audit-team/` | v1.5.0 | `audit-team/v1.5.0` |
| **bandit** | `bandit/` | v1.0.1 | `bandit/v1.0.1` |
| **handoff** | `handoff/` | v1.0.2 | `handoff/v1.0.2` |
| **plan-my-grill** | `plan-my-grill/` | v1.6.0 | `plan-my-grill/v1.6.0` |
| **preflight** | `preflight/` | v1.0.0 | `preflight/v1.0.0` |
| **python-radon** | `python-radon/` | v1.0.0 | `python-radon/v1.0.0` |
| **report** | `report/` | v1.1.0 | `report/v1.1.0` |
| **slop-team-six** | `slop-team-six/` | v2.0.1 | `slop-team-six/v2.0.1` |
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
