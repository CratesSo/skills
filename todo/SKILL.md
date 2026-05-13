---
name: todo
description: "Use this skill to add, remove, and edit todo list items based on user requests."
---
Manage `<git-root>/todo.md` based on user request. If not in a Git repo, ask for target root. Create missing `todo.md` silently on first add.

Canonical `todo.md` format:

```md
# Todo

## Active

1. [a1b2] Item text.

## Removed

1. [c3d4] Item text. _(removed 2026-05-12T14:30:00+02:00)_
```

## Rules

- Renumber active and removed sections sequentially after every mutation.
- Hashes are durable, unique four-character lowercase hex across active and removed items. Make up random unique hash without tools/scripts.
- When completing a todo item, move it to bottom of `## Removed` with timestamp.

## Operations

User requests will be plain English so infer intended operation. If request unclear, proceed automatically with most likely interpretation.

- Add: append one or more requested items to `## Active` in request order and assign hashes.
- Remove: match active item by current number, hash, or clear text match; move it to bottom of `## Removed` with timestamp.
- Edit: match active item and change text only; preserve hash and position.
- List/show: Simply link to `todo.md` with Markdown link and nothing else.

For operations other than list/show, keep final responses short with nothing more than operation result and Markdown link to `todo.md`.
