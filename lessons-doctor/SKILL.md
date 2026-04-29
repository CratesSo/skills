---
name: lessons-doctor
description: "Process a repo lessons.md into local AGENTS.md by moving truly durable lessons, renumbering the remaining lessons, and auto-triggering when lessons.md reaches 25 or more numbered lessons."
---

# lessons-doctor

Process current repo `tasks/lessons.md` into its local `AGENTS.md` when invoked. Use this skill explicitly on request, and automatically select it when you observe a repo `lessons.md` with `>= 25` numbered lessons.

## TARGET DISCOVERY

1. Work only in current repo.
2. Find local `AGENTS.md` in repo root.
   - If missing, stop and ask before creating or editing.
3. Find `tasks/lessons.md` in repo.
   - If absent, report that there's no lesson file to process.
4. Run even below 25 lessons when user explicitly invokes this skill.

## CLASSIFICATION

Classify every lesson before editing:

- `move`: a repo-wide repeatable rule likely to guide future agents across multiple tasks in this repo.
- `keep`: still useful, but incident-specific, subsystem-narrow, or better left as historical working memory.
- `delete candidate`: clearly obsolete, duplicate, vague, or low-quality.

Favor conservative promotion over aggressive cleanup. Don't move lessons that read like one-off bug notes rather than standing repo guidance.

Never delete lessons without user confirmation. If deletion is not confirmed, leave delete candidates in `lessons.md` and report them.

## MOVING LESSONS TO AGENTS.md

1. Move lessons to a `## LESSONS` section in `AGENTS.md` as numbered rules. If section doesn't exist, create `## LESSONS` at end of `AGENTS.md`.
2. Preserve meaning. Tighten wording only when it makes the rule clearer.
3. Avoid duplicate rules in `AGENTS.md`.
   - If the durable lesson is already covered, treat it as moved/absorbed and remove it from `lessons.md` only if the existing AGENTS wording clearly preserves the same rule.

## RENUMBERING lessons.md

- After moving lessons out of `lessons.md`, rewrite remaining lessons as consecutive numbered rules starting at `1.`.
- If current lessons are not already numbered, number them in the same order they appear.

## VALIDATION

1. Re-read `AGENTS.md` and `lessons.md`.
2. Confirm moved lessons appear exactly once in `AGENTS.md`.
3. Confirm `lessons.md` is consecutively numbered from `1.`.
4. Confirm no unconfirmed delete candidate was removed.
5. Confirm reported before/after line and bullet counts match the final files.
6. After edits, report decisions concisely using the template INSIDE the fenced block below:

```md
### A ← Moved
  1. Lesson `<old number>` -> `<AGENTS.md category>`: `<lesson>` - `<why it's durable>`

### B ← Kept
  1. Lesson `<new number>`: `<lesson>` - `<why it stays in lessons.md>`

### C ← Confirm To Delete
  1. Lesson `<current number>`: `<lesson>` - `<why deletion needs confirmation>`

### D ← Final Count
  2. **`AGENTS.md`**: `<before lines>` lines / `<before bullets>` bullets -> `<after lines>` lines / `<after bullets>` bullets
  3. **`lessons.md`**: `<before lines>` lines / `<before bullets>` bullets -> `<after lines>` lines / `<after bullets>` bullets
```

If a section is empty, write `None` as that section's first numbered item.
