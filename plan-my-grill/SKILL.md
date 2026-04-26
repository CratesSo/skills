---
name: plan-my-grill
description: "Interrogate plans and designs with structured questions until they are handoff-ready."
---
# plan-my-grill

Interrogate a plan or design until handoff ready and no high impact blockers remain:

## BEFORE STARTING

First prompt must batch `2` tightly coupled questions using `request_user_input`:

- Ask user `How many prompts do you want?`:
  - `15-20`
  - `10-15`
  - `5-10`
- Ask first real intent question in that same prompt:
  - `goal`+`audience`

Second prompt must happen immediately after opener using `request_user_input`:

- `success criteria`
- `scope boundaries`
- `constraints`

Default Targets:

- `5-10` => `7-8`
- `10-15` => `12-13`
- `15-20` => `17-18`

Example: User sets `5-10`, plan for `7-8` prompts unless plan becomes complete sooner or needs more by prompt `8`.

- Prompt count tracks prompts, not raw question count. A batched prompt counts as `1` prompt.
- After opener is answered, ask second batched prompt immediately. Never pause just because prompt count choice was answered.

## START WITH INTENT

- `goal`+`audience`
- `success criteria`
- `scope boundaries`
- `constraints`

## AFTER INTENT LOCKED

Use a hidden completeness frame and track what still matters like interface, data flow, failure modes, UX, rollout, etc.

- Ask the next question based on what removes the highest impact blocker.
- Route questions by blocker severity over following rigid checklist.

## QUESTION RULES

- Reaching initial target never auto stops. Continue to selected range top if high impact blocker remains.
- If question can be answered by exploring code or environment, explore over asking.
- Never ask low risk detail questions unless detail is likely to distort plan or implementation.
- If something is underspecified, ask unless assumption is low risk or unlikely to change implementation materially.
- Track resolved decisions, never repeat or lightly rephrase answered questions.
- Revisit resolved points only if new info creates inconsistency or invalidates earlier answer.
- Call out weak, inconsistent, or underspecified choices directly.
  - Force real tradeoff decision if weak assumption would materially change implementation.
  - Never press low impact disagreements if risk is understood and choice locked.
- Always add a fire 🔥 emoji instead of "(recommended)" directly after the user selectable option you most recommend for each question.

## QUESTION STYLE

Use `request_user_input` for questions that materially change plan, confirm important assumptions, or choose between real tradeoffs:

- Default to one high impact question at a time.
- Ask `2-3` questions in one prompt if they're tightly coupled and batching can close branches faster than asking one at a time.
- Hard Exception: The first `2` prompts are always mandatory batched prompts.
- First Prompt = `prompt-count selector`+`goal`+`audience`.
- Second Prompt = `success criteria`+`scope boundaries`+`constraints`.
- Offer `3-4` meaningful mutually exclusive options.

## CONTINUATION

Ask "Keep going?" with three options at user selected range top:

- Count the two mandatory opening batched prompts toward that selected prompt range.
- `Keep going`: continue asking as before
- `Stop here`: produce final plan
- `Three more`: ask three more, then ask "Keep going?" again

## STOP

Stop if no high impact unresolved blockers remain or user says to stop.

## DONE

When questioning done, produce full handoff-ready `<proposed_plan>` complete on intent, implementation, tests, assumptions, avoid leaving decisions to implementer.
