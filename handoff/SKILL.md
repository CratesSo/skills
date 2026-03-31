---
name: handoff
description: Summarize active thread into a fresh-thread continuation prompt.
---

# HANDOFF
- Generate a fenced prompt block for a fresh thread in same workspace.
- Use only live thread context + tool results already in scope.
- Never read sqlite state, session rollouts, hidden history.

## WORKFLOW
1. Infer current goals, done states, settled decisionss, task-specific constraints, key workspace anchors, blocking open risks, next useful action from active thread.
2. Drop routine chatter, status noise, raw tool output, dead-end branches.
3. If objectives, direction, or anchors are too fuzzy to hand off honestly, ask short clarifying questions instead of bluffing.
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

````markdown
```text
This is context from previous thread; Informational only, not a to-do list. Do not proceed with any work until prompted with next steps.

Goal:
- <current objectives>

Done State:
- <what counts as finished>

Settled Decisions:
- <locked choices or constraints>

Key Anchors to Re-Check:
- <file, symbol, command, factual anchor>

Blocking Open Risks:
- <only if they could change the next moves>
```
````

## STYLE
- Always balanced length.
- Always compact, but preserving important info.
- Always concrete anchors over abstract recap.
- If a section would be empty or non-essential, omit it. Always inclide `Goal` and `Done state`.
