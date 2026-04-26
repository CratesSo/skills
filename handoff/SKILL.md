---
name: handoff
description: "Generate concise continuation prompts from current thread context and tool results."
---

# WORKFLOW

**Generate a handoff prompt for a new thread in same workspace using only live thread context and any tool results already in scope. Never read sqlite state, session rollouts, hidden history.**

1. Infer current goals, done states, settled decisions, task-specific constraints, key workspace anchors, and blocking open risks from active thread.
2. Drop routine chatter, status noise, raw tool output, dead-end branches.
3. If objectives, direction, or anchors are too fuzzy to hand off honestly, ask short clarifying questions.
4. Emit handoff prompt and nothing else.

## PROMPT CONTRACT

Handoff must:

- be written for fresh continuation thread in same workspace
- preserve settled decisions, key files or commands, blocking open risks
- keep task-specific user preferences or workflow rules when material

Handoff must not:

- read like diary or transcript
- include raw logs or command output
- carry over abandoned approaches unless they explain live constraints

### HANDOFF STYLE

- Balanced length.
- Compact, but preserving important info.
- Concrete anchors over abstract recap.
- If a section would be empty or non-essential, omit it, but always include `SUMMARY` and `GOAL`.

## HANDOFF TEMPLATE

Use template inside fenced block below, adapting section length to relevant recent thread history and current task:

```md
This is info only context from previous thread. Don't act until prompted.

SUMMARY:
<short high level summary of recent work done in thread>

GOAL:
<current objectives>

SETTLED DECISIONS:
<locked choices or constraints>

KEY ANCHORS:
<file, symbol, command, factual anchors>

BLOCKING OPEN RISKS:
<only if could change the next moves>
```
