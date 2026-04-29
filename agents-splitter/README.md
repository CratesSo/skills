# agents-splitter
- Audit large AGENTS.md files for reference-file extraction opportunities.
- Split confirmed sections into adjacent AGENTS_<topic>.md files while preserving meaning.

## How to use
1. Invoke the skill and choose either the global AGENTS.md or the repo-root AGENTS.md target.
2. Review the audit report, then confirm any extraction candidates you want applied.

### Examples
- `Use agents-splitter to audit this repo AGENTS.md for split-out references.`
- `Use agents-splitter on my global AGENTS.md and apply the confirmed extractions.`

## Version

Current version: v1.0.2

## Install
`npx skills@latest add CratesSo/skills/agents-splitter`
