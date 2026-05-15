---
name: handoff
description: "Generate concise continuation prompts from current thread context and tool results."
---
# WORKFLOW
Use live thread context and tool results already in scope to summarize the conversation, current goal(s), settled decisions, key anchors, and blockers into a handoff for a new thread.

If invoked as `$handoff run`:
1. Fully read repo-root `HANDOFF.md` to use as current-thread context. If missing, reply `No HANDOFF.md found`.
2. Delete `HANDOFF.md`.
3. Reply `Ready`.

If invoked as `$handoff`:
1. Generate handoff using template in `references/handoff-template.md`, omitting any sections not relevant/immaterial.
2. Overwrite repo-root `HANDOFF.md` with new handoff.
3. Reply `Saved HANDOFF.md`.
