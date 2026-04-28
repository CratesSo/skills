---
name: lessons
description: "Process a repo lessons.md into local AGENTS.md by moving truly durable lessons, renumbering the remaining lessons, and auto-triggering when lessons.md reaches 25 or more numbered lessons."
---

# lessons

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

Never delete lessons without user confirmation when using this skill. If deletion is not confirmed, leave delete candidates in `lessons.md` and report them.

## MOVING LESSONS INTO AGENTS.md

1. Preserve meaning. Tighten wording only when it makes the rule clearer.
2. Add moved lessons to a `## LESSONS` section in `AGENTS.md`. If section doesn't exist, create `## LESSONS` at end of `AGENTS.md`.
   - Add moved lessons there as numbered rules.
3. Avoid duplicate rules in `AGENTS.md`.
   - If the durable lesson is already covered, treat it as moved/absorbed and remove it from `lessons.md` only if the existing AGENTS wording clearly preserves the same rule.

## RENUMBERING lessons.md

- After moving lessons out of `lessons.md`, rewrite remaining lessons as consecutive numbered rules starting at `1.`.
- If current lessons are not already numbered, number them in the same order they appear.

## REPORTING CONTRACT

After edits, report decisions concisely using the template INSIDE the fenced block below:

```md
## MOVED
- `<old number>` -> `<AGENTS.md category>`: `<lesson>` - `<why it's durable>`

## KEPT
- `<new number>`: `<lesson>` - `<why it stays in lessons.md>`

## DELETE CANDIDATES
- `<current number>`: `<lesson>` - `<why deletion needs confirmation>`

## FINAL COUNT
- `lessons.md`: `<count>` lessons
```

If a section is empty, say `None` under that heading.

## VALIDATION

After editing:

1. Re-read `AGENTS.md` and `lessons.md`.
2. Confirm moved lessons appear exactly once in `AGENTS.md`.
3. Confirm `lessons.md` is consecutively numbered from `1.`.
4. Confirm no unconfirmed delete candidate was removed.
