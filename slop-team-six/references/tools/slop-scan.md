# Slop Scan Tool Reference

Use this during preflight and as a Lane 3 evidence source.

## Purpose

Run the hooky slop scanner to find scanner-backed candidates for AI slop, stubs, low-value comments, and cleanup warnings.

Scanner output is evidence only. Never edit files just because the scanner found issues.

## Scope

- Default to repo-wide scans unless the user specifies a narrower scope.
- For targeted scans, run from the smallest target directory.
- The scanner does not take file arguments.

## Command

Run from the repo root or smallest target directory:

```bash
python3 /Users/admin/Desktop/codex/hooky/hooks/slop_scan.py
```

## Tool Policy

- Install missing helper tools such as `semgrep`, `ast-grep`, `shellcheck`, or `shfmt` only when the parent handoff explicitly approves the install command and the helper materially improves coverage without changing the target repo.
- Report helpers that still cannot be installed or run as coverage gaps.
- Do not probe `--help`; unsupported flags can start a normal scan instead of returning usage.
- Keep output compact and capped; rerun narrower scans only when needed.

## Lane 3 Use

Use scanner findings to seed Lane 3 investigation. A Wave 1 agent must still prove:

- the exact target and location
- why it is low-value, misleading, stub-like, or redundant
- whether deletion has behavior impact
- any readability or workflow risk

The parent must triage scanner findings before Wave 2 edits.

## Tool Output To Summarize

Summarize tool output in this order:

1. Summary counts: files scanned, files skipped, blocker count, warning count, runner error count, missing tools.
2. `BLOCKERS`: hard failures from the hook script.
3. `WARNINGS`: advisory warnings from the hook script.
4. `RUNNER ERRORS`: only if the scanner itself failed on a file.
5. Coverage notes.
