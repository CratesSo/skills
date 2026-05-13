---
name: todo
description: "Manage a repo-root todo.md with durable four-character item references."
---

# todo

Manage `<git-root>/todo.md`. If not in a Git repo, ask for the target root. Create missing `todo.md` silently on first add.

Canonical format:

```md
# Todo

## Active

1. [a1b2] [ ] Item text

## Removed

1. [c3d4] Item text _(removed 2026-05-12T14:30:00+02:00)_

## Notes
```

## Rules

- Active lines: `N. [hash] [ ] text`.
- Removed lines: `N. [hash] text _(removed local-ISO-8601-timestamp)_`.
- Renumber active and removed sections sequentially after every mutation.
- Hashes are unique four-character lowercase hex across active and removed items.
- Generate random hashes; retry generated collisions silently. Ask on user-supplied duplicate hashes.
- V1 excludes done workflows, priorities, labels, due dates, sync, insertion positions, and reordering.

## Operations

- Add: append one or many requested items to `## Active` in request order, assign hashes, then report new refs.
- Remove: match active item by current number, hash, or clear text match; move it to bottom of `## Removed` with timestamp.
- Edit: match active item and change text only; preserve hash and position.
- List/show: link to `todo.md` and summarize active refs; dump full list only when explicitly requested. Show removed items only when requested.

If text matching has multiple plausible candidates, ask with number, hash, and short text.

## Normalization

If existing `todo.md` is non-canonical, normalize before the requested operation:

- Convert obvious numbered, bulleted, and checkbox todo lines into active items.
- Preserve valid existing hashes; assign hashes where missing.
- Preserve unrelated prose/headings exactly under `## Notes`.
- Write one canonical `# Todo` document with `## Active`, `## Removed`, and `## Notes`.

Keep final responses short: operation result, changed refs, and `todo.md` location.
