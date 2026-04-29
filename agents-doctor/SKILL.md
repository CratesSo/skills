---
name: agents-doctor
description: "Audit local repo AGENTS.md for safe cleanup opportunities, then optionally apply previously reported safe changes."
---

# agents-doctor

Audit current repo local `AGENTS.md` for duplicate, redundant, repetitive, verbose, stale, or low-value guidance. Always report first. Never edit on initial invocation.

## TARGET DISCOVERY

- Work only in current repo.
- Find repo-root `AGENTS.md`.
- If no local `AGENTS.md`: Stop, report that there's no local `AGENTS.md`, offer a creation plan based on repo facts only if useful, and don't create file unless user confirms.

## CANONICAL WORKFLOW

### 1. Audit and Report

- 1a. Fully read local `AGENTS.md` and produce an audit report.
- 1b. Ask if user wants to apply the safe changes. If yes, proceed to step 2. If no, stop.

### 2. Apply Safe Changes

- 2a. Apply only the safe changes listed in the previous report under `MERGE`, `COMPRESS`, and `DELETE`.
- 2b. Use best judgment for meaning-preserving compression.
- 2c. Never apply anything listed under `NEEDS CONFIRMATION` unless user confirmed that item.
- 2d. If the current file drifted enough that a listed safe change no longer matches cleanly, skip that item and report it.

## AUDIT PRIORITY

Classify cleanup opportunities in this order:

1. REDUNDANCY:
   - Exact duplicates of other rules or sections
   - Rules covered by stronger local or global instructions
   - Overlap between sections that can be merged
   - Repeated concepts that can be expressed once
2. BREVITY:
   - Unnecessarily verbose bullets that can be compressed without losing meaning
   - Repeated qualifiers or conditions that can be stated once
   - Long section wording that can be compressed without changing meaning
   - Repetitive examples that can be reduced to one or two representative cases
   - Repeated formatting or structure that can be simplified (e.g. multiple nested lists that can be flattened)
   - Repetitive references to the same tools, files, or paths that can be consolidated
3. FRESHNESS:
   - Stale paths or file references that no longer exist or are no longer relevant
   - Obsolete tools or versions that are no longer used or supported
   - Outdated workflow steps that have changed due to recent repo updates
   - Rules contradicted by current repo facts or recent changes
4. LOW VALUE:
   - Guidance too generic to be actionable, too specific to be broadly useful, or only relevant to a past state of the repo and unlikely to be relevant again
   - Rules that are only relevant to a specific edge case that is unlikely to occur again
   - Guidance that is already well-covered by global behavior and doesn't materially add to it

## PRESERVE BY DEFAULT

- Repo-specific invariants and constraints
- Hard-won behavior rules
- Signing, install, sandbox, validation, and workflow workarounds that required product or architectural judgment to create
- File and path anchors that materially help future agents navigate the repo
- Rules whose deletion would require product or architectural judgment to replace or re-anchor
- Rules that are still relevant and not stale, even if they could be compressed or merged

## CLASSIFICATION

- `merge`: content should move into another existing section or combine with a nearby rule
- `compress`: content is useful but wordier than needed
- `remove`: content safe to remove because it's an exact duplicate, clearly covered duplicate, or generic rule already covered by global behavior
- `needs confirmation`: content may be low-value, stale, or compressible, but removing or changing it could alter meaning, priority, or repo-specific intent
- `keep`: content remains valuable enough to preserve

If unsure, use `needs confirmation` over `remove`.

## REPORTING CONTRACT

On the audit turn, report decisions using the template INSIDE the fenced block below:

```md
## MERGE
- **`<section/item>`** -> **`<target section>`**: `<concise reason>`

## COMPRESS
- **`<section/item>`**: `<old issue>` -> `<proposed shorter wording>`

## REMOVE
- **`<section/item>`**: `<concise reason>` (e.g. exact duplicate of `<other section/item>`, covered by global behavior, or clearly redundant with `<other section/item>`)

## NEEDS CONFIRMATION
- **`<section/item>`**: `<concise reason>`

## KEEP
- **`<section/item>`**: `<concise reason>`

## CURRENT COUNT
- **`<line count>`** lines, **`<bullet count>`** bullets
```

If a section is empty, write `None`.

## AFTER APPLYING CHANGES

1. Fully re-read `AGENTS.md`.
2. Confirm intended sections still exist.
3. Confirm applied safe duplicates and redundancies are gone.
4. Confirm no unconfirmed `NEEDS CONFIRMATION` item was removed.
5. Return final output using the template INSIDE the fenced block below (skipping any section without changes):

```md
## Merged
- **`<section/item>`** -> **`<target section>`**: `<concise reason>`

## Compressed
- **`<section/item>`**: `<old issue>` -> `<proposed shorter wording>`

## Removed
- **`<section/item>`**: `<concise reason>` (e.g. exact duplicate of `<other section/item>`, redundant with `<other section/item>`, etc.)

## Line Count
- **Before**: `<line count>` lines, `<bullet count>` bullets
- **After**: `<line count>` lines, `<bullet count>` bullets
```
