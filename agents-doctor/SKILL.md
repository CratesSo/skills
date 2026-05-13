---
name: agents-doctor
description: "Maintain AGENTS.md through lessons, cleanup, and optional splitting."
---

# agents-doctor

Maintain `AGENTS.md` through lessons, cleanup, and optional splitting.

Run phases in this exact order:

1. Process durable lessons.
2. Clean up `AGENTS.md`.
3. Split large situational guidance.

## Target Discovery

- Determine the repo root with `git rev-parse --show-toplevel` when available.
- If not inside a git repo, use the current working directory as the target root and report that assumption.
- Find `AGENTS.md` at the target root.
- If the user explicitly asks for global AGENTS, use `/Users/admin/.codex/AGENTS.md` and skip repo `lessons.md` unless the user gives another lesson file.
- If the target `AGENTS.md` is missing, stop, report the missing path, and do not create it unless the user confirms.
- If repo `lessons.md` is absent, report that the lessons phase was skipped and continue to cleanup.

## Default UX

- Default invocation is audit-first. Do not edit files unless the user explicitly asks to apply changes.
- Run phases sequentially and report proposed changes before applying any phase.
- If the user asks to apply changes, apply only the confirmed items from the latest report.
- If an apply request is ambiguous, apply only clearly named phases or items; otherwise ask before editing.
- Never delete lessons without explicit user confirmation.
- Keep edits narrow and local to `AGENTS.md`, `lessons.md`, and generated `AGENTS_<topic>.md` reference files.

## Phase 1: Lessons

Classify every lesson in repo `lessons.md` before editing:

- `move`: repo-wide repeatable guidance likely to help future agents across multiple tasks.
- `keep`: useful but incident-specific, subsystem-narrow, historical, or not broad enough for `AGENTS.md`.
- `delete candidate`: obsolete, duplicated, vague, or low-value. Report only unless the user confirms deletion.

Report lesson edits:

1. Propose `move` lessons for a `## LESSONS` section in `AGENTS.md`, creating the section at the end only if the user confirms.
2. If a lesson is already clearly covered in `AGENTS.md`, report it as an absorb candidate.
3. Propose tighter wording only when it clearly improves reuse.
4. Report the resulting `lessons.md` renumbering that would happen after confirmed moves.

When the user confirms lesson edits, apply only confirmed moves or absorptions, renumber remaining `lessons.md` lessons consecutively from `1.`, and validate moved guidance appears exactly once.

## Phase 2: Cleanup

Audit `AGENTS.md` after the lessons phase. In audit-only mode, use the current file plus proposed lesson changes as context; after confirmed lesson edits, use the updated file.

1. Redundancy: exact duplicates, overlap, repeated concepts, or rules covered by stronger local guidance.
2. Brevity: wording that can be compressed without losing meaning.
3. Freshness: stale paths, obsolete tools, outdated workflow steps, or repo facts contradicted by current files.
4. Low value: generic, past-state, or edge-case guidance that no longer earns always-loaded space.

Classify findings:

- `merge`: combine overlapping content into one canonical place.
- `compress`: shorten useful content without changing meaning.
- `remove`: delete exact duplicates, clearly covered duplicates, or generic guidance already covered by the same target `AGENTS.md` or stronger active instructions.
- `needs confirmation`: changing it could alter meaning, priority, or repo-specific intent.
- `keep`: preserve because it remains useful or deletion would require judgment.

Report `merge`, `compress`, and `remove` findings as safe candidates. Do not apply anything during the default audit.

When the user confirms cleanup edits, apply only the confirmed findings.

Preserve by default:

- Repo-specific invariants and constraints.
- Hard-won behavior rules.
- Tool, signing, install, sandbox, validation, and workflow workarounds.
- File and path anchors that help future agents navigate the repo.
- Safety, permission, and destructive-command constraints.

## Phase 3: Splitting

Audit the final intended `AGENTS.md` for large situational guidance that can move to adjacent reference files without changing meaning. In audit-only mode, use the current file plus proposed lesson and cleanup changes as context; after confirmed edits, use the updated file.

Good split candidates include repo-type guidance, work-mode runbooks, tool-specific procedures, rare workflows, and long examples that are not needed for most tasks.

Do not split core rules, safety constraints, target discovery rules, short guidance where the pointer costs as much context as the original, or content whose meaning depends on nearby priority/order.

Score each split candidate from `1` to `5` to prioritize the report:

- `context savings`: expected reduction in always-loaded context.
- `lookup clarity`: how obvious it is when to open the reference.
- `preservation risk`: risk that moving changes meaning, priority, or usage.

Do not create reference files during the default audit. Report split candidates and require explicit confirmation before applying them.

For each confirmed split:

1. Create `AGENTS_<topic>.md` beside the target `AGENTS.md`, using a lowercase underscore slug from the original section title.
2. Move the section nearly verbatim into the reference file.
3. Replace the original section with a concise pointer that includes the absolute reference path and a specific trigger condition.
4. Ensure the moved guidance exists in one canonical location only.

Report all non-confirmed candidates as `needs confirmation` or `skipped`.

## Apply Validation

After applying any confirmed edits:

1. Re-read every touched file.
2. Confirm no unconfirmed item was changed.
3. Confirm moved lesson or split guidance exists in one canonical place.
4. Confirm every generated pointer uses the correct absolute path and resolves to an existing file.
5. Confirm reported line and bullet counts match the final files.

## Final Report

Return a compact phase ledger:

```md
### A ← Lessons
  1. **Proposed**: `<items or None>`
  2. **Applied**: `<items or None>`
  3. **Kept**: `<items or None>`
  4. **Confirm To Delete**: `<items or None>`

### B ← Cleanup
  1. **Proposed**: `<items or None>`
  2. **Applied**: `<items or None>`
  3. **Needs Confirmation**: `<items or None>`

### C ← Splits
  1. **Proposed**: `<items or None>`
  2. **Applied**: `<AGENTS_<topic>.md files or None>`
  3. **Skipped / Needs Confirmation**: `<items or None>`

### D ← Counts
  1. **AGENTS.md**: `<before>` lines / `<before>` bullets -> `<after>` lines / `<after>` bullets
  2. **lessons.md**: `<before>` lines / `<before>` bullets -> `<after>` lines / `<after>` bullets, `skipped`, or `skipped (global AGENTS target)`
```

If a phase made no changes, say so plainly. Include validation failures or skipped items in the relevant section.
