---
name: lessons
description: "Process a repo lessons.md into local AGENTS.md by moving truly durable lessons, renumbering the remaining lessons, and auto-triggering when lessons.md reaches 20 or more numbered lessons."
---

# lessons

Process the current repo's `lessons.md` into its local `AGENTS.md` when invoked. Use this skill explicitly on request, and automatically select it when you observe a repo `lessons.md` with `>= 20` numbered lessons.

This is not a background watcher. The auto-trigger is an agent behavior: when the condition is visible during a turn, load and run this skill.

## TARGET DISCOVERY

1. Work only in the active/current repo.
2. Find local `AGENTS.md` at the repo root.
   - If missing, stop and ask before creating or editing anything.
3. Find `**/lessons.md` in the active repo.
   - Exclude generated, build, cache, vendor, dependency, and lockstep artifact paths.
   - If none exist, report that there is no lesson file to process.
   - If multiple exist, stop and ask which one to process.
4. Run even below 20 lessons when the user explicitly invokes this skill.

## CLASSIFICATION

Classify every lesson before editing:

- `move`: a repo-wide repeatable rule likely to guide future agents across multiple tasks in this repo.
- `keep`: still useful, but incident-specific, subsystem-narrow, or better left as historical working memory.
- `delete candidate`: clearly obsolete, duplicate, vague, or low-quality.

Favor conservative promotion over aggressive cleanup. Do not move lessons that read like one-off bug notes rather than standing repo guidance.

Never delete lessons without explicit user confirmation in that run. If deletion is not confirmed, leave delete candidates in `lessons.md` and report them.

## MOVING LESSONS INTO AGENTS.md

1. Preserve meaning. Tighten wording only when it makes the rule clearer.
2. Prefer an existing relevant/matching `AGENTS.md` category.
   - Add moved lessons there as bullets.
3. If no relevant category exists, create `## LESSONS` near the end of `AGENTS.md`.
   - Add moved lessons there as numbered rules.
4. Avoid duplicate rules in `AGENTS.md`.
   - If the durable lesson is already covered, treat it as moved/absorbed and remove it from `lessons.md` only if the existing AGENTS wording clearly preserves the same rule.
5. Do not process nested repos or unrelated workspaces.
6. Do not edit generated, build, cache, vendor, dependency, or lockstep artifact paths.

## RENUMBERING lessons.md

After moving confirmed durable lessons out of `lessons.md`, rewrite the remaining lessons as consecutive numbered rules starting at `1.`.

Keep remaining lesson wording unchanged unless a tiny grammar fix is required by the renumbering edit.

## REPORTING CONTRACT

After edits, report every decision concisely and directly:

```md
## MOVED
- `<old number>` -> `<AGENTS.md category>`: `<lesson>` - `<why it is durable>`

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

1. Re-read `AGENTS.md` and the selected `lessons.md`.
2. Confirm moved lessons appear exactly once in `AGENTS.md`.
3. Confirm `lessons.md` is consecutively numbered from `1.`.
4. Confirm no unconfirmed delete candidate was removed.
