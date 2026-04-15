---
name: handoff
description: User triggered.
---
Generate fenced prompt block for new thread in same workspace using only live thread context and any tool results already in scope. Never read sqlite state, session rollouts, hidden history.

# WORKFLOW
1. Infer current goals, done states, settled decisions, task-specific constraints, key workspace anchors, and blocking open risks from active thread.
2. Drop routine chatter, status noise, raw tool output, dead-end branches.
3. If objectives, direction, or anchors are too fuzzy to hand off honestly, ask short clarifying questions.
4. Emit a fenced prompt block inside a single code block and nothing else.

## PROMPT CONTRACT
Prompt block must:
- be written for fresh thread in same workspace
- preserve settled decisions, key files or commands, blocking open risks
- keep task-specific user preferences or workflow rules when they materially affect task

Prompt block must not:
- read like diary or transcript
- include raw logs or command output
- carry over abandoned approaches unless they explain live constraints

## TEMPLATE
Use this shape, adapting section length to task:

```text
This is context from previous thread. Info only. Don't proceed with work until prompted.

GOAL:
- <current objectives>

DONE STATE:
- <what counts as finished>

SETTLED DECISIONS:
- <locked choices or constraints>

KEY ANCHORS:
- <file, symbol, command, factual anchor>

BLOCKING OPEN RISKS:
- <only if they could change the next moves>
```

### STYLE
- Always balanced length.
- Always compact, but preserving important info.
- Always concrete anchors over abstract recap.
- If a section would be empty or non-essential, omit it, but always include `Goal` and `Done state`.
