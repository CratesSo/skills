---
name: agents-splitter
description: "Audit large AGENTS.md files for reference-file extraction opportunities, then optionally split confirmed sections into adjacent AGENTS_<topic>.md files."
---

# agents-splitter

Audit large `AGENTS.md` files for meaning-preserving extraction opportunities, then optionally split confirmed sections into adjacent `AGENTS_<topic>.md` reference files. Always report first. Never edit on initial invocation.

## TARGET SELECTION

Ask which target to audit before reading files:

- Global AGENTS: `/Users/admin/.codex/AGENTS.md`
- Repo-root AGENTS: `<current repo>/AGENTS.md`

If selected target doesn't exist, stop and report missing path. Don't create a target `AGENTS.md` unless user explicitly asks.

## CANONICAL WORKFLOW

### 1. Audit and Report

1. Fully read selected `AGENTS.md`.
2. Identify large situational guidance that can move to reference files without changing meaning.
3. Report extraction candidates and their scores using the `## REPORTING CONTRACT` section below.
4. Ask if user wants to apply any listed extractions.
5. If user doesn't confirm an apply step, stop after audit.

### 2. Apply Confirmed Extractions

1. Apply only the extraction candidates the user confirmed from the previous audit.
2. Create each reference file beside the selected `AGENTS.md` using `AGENTS_<topic>.md`.
3. Derive `<topic>` from source section heading as lowercase slug with words separated by underscores.
4. Move confirmed section content nearly verbatim into reference file.
5. Replace only moved content in `AGENTS.md` with concise pointer that states when to read the reference and includes its absolute path.
6. Leave all unrelated `AGENTS.md` content unchanged.
7. Don't compress, rewrite, reorder, or reorganize unrelated guidance.

## EXTRACTION CRITERIA

Extract only guidance that's both large enough to create meaningful context savings and situational enough that most agents should not read it by default.

Good candidates:

- Repo-type guidance, such as frontend, Python, mobile, infrastructure, or data work
- Work-mode guidance, such as code review, release, migration, incident, or design work
- Tool-specific runbooks that apply only when using that tool
- Rare workflow instructions that are valuable but not needed for normal tasks
- Long examples or detailed procedures that can be loaded on demand

Poor candidates:

- Core rules every agent must follow before doing any work
- Safety, permission, sandbox, or destructive-command constraints
- Target discovery rules needed to choose which reference to read
- Short guidance where a pointer would cost as much context as the original text
- Content whose meaning depends on nearby global priority or ordering

If unsure whether extraction changes priority, classify as `needs confirmation` instead of `extract`.

## SCORING

Score each candidate from `1` to `5` in these categories:

- `context savings`: expected reduction in always-loaded `AGENTS.md` context
- `lookup clarity`: how obvious it is when an agent should read the reference
- `preservation risk`: risk that moving the section changes meaning, priority, or usage

Use `low`, `medium`, or `high` for overall priority. Prefer fewer high-confidence candidates over broad noisy lists.

## POINTER CONTRACT

The replacement text in `AGENTS.md` must be short and explicit. Use the shape INSIDE the fenced block below, unless the surrounding file has a clearly better local style:

```md
## <Original Section Title>

For <trigger condition>, read and follow `<absolute path to AGENTS_<topic>.md>`.
```

The trigger condition must be specific enough that a future agent knows when to open the reference file.

## REFERENCE FILE CONTRACT

Each created reference file must:

- Keep the moved section title or a directly equivalent title
- Preserve moved rules nearly verbatim
- Include enough heading context to stand alone when opened directly
- Avoid adding new policy, new examples, or speculative cleanup
- Avoid duplicating content that remains in `AGENTS.md`, except for the title needed to anchor the document

## REPORTING CONTRACT

On the audit turn, report using the template INSIDE the fenced block below:

```md
## TARGET
- **Path**: `<absolute AGENTS.md path>`
- **Current count**: `<line count>` lines, `<bullet count>` bullets

## EXTRACT
- **`<section title>`** -> **`<absolute AGENTS_<topic>.md path>`**
  - **Priority**: `<high|medium|low>`
  - **Scores**: context savings `<1-5>`, lookup clarity `<1-5>`, preservation risk `<1-5>`
  - **Reason**: `<why this is situational and worth extracting>`
  - **Pointer**: `<exact replacement text for AGENTS.md>`

## NEEDS CONFIRMATION
- **`<section title>`**: `<why extraction might change meaning, priority, or usage>`

## KEEP IN AGENTS.md
- **`<section title>`**: `<why this should remain always loaded>`
```

If a section is empty, write `None`.

## AFTER APPLYING CHANGES

1. Fully reread `AGENTS.md` and created `AGENTS_<topic>.md` file(s).
2. Confirm every moved section exists in one canonical place.
3. Confirm `AGENTS.md` includes each expected pointer with correct absolute path.
4. Confirm unrelated `AGENTS.md` sections were not changed.
5. Report using the template INSIDE the fenced block below:

```md
## Changed Files
- `<absolute path>`

## Extracted
- **`<section title>`** -> **`<absolute AGENTS_<topic>.md path>`**

## Skipped
- **`<section title>`**: `<reason>`

## Validation
- **Moved guidance represented once**: `<yes|no>`
- **Pointers include absolute paths**: `<yes|no>`
- **Unrelated AGENTS.md content unchanged**: `<yes|no>`

## Line Count
- **Before**: `<line count>` lines, `<bullet count>` bullets
- **After**: `<line count>` lines, `<bullet count>` bullets
```

If a section is empty, write `None`.

## HARD RULES

- Preserve meaning by default.
- Keep one canonical location for extracted guidance.
- Never leave both full original section and new reference file in place.
- Never add fallback paths, duplicate pointers, summaries, indexes, or extra structure unless needed to preserve meaning.
