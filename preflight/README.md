# preflight
- Run a production-readiness audit before launch.
- Checks security, database, deployment, and code evidence without marking unknowns as passes.

## How to use
1. Type `$preflight <repo or launch target>`.
2. Review the blocker, warning, passed, and unknown findings before deciding on fixes.

### Examples
- `$preflight check this app before launch`
- `$preflight audit production readiness for the API`

## Version

Current version: v1.0.0

## Install
`npx skills@latest add CratesSo/skills/preflight`
