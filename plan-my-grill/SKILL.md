---
name: plan-my-grill
description: "Interrogate plans and designs with structured questions until they are handoff-ready."
---
Interrogate a plan or design until handoff ready and no high impact blockers remain.

## OPENING SEQUENCE

LOCK intent through opening sequence before moving into adaptive questioning:

`Prompt 1` must batch the depth selector with the first intent question:
- Ask user `How deep should this planning pass go?`:
  - `Quick`: core blockers
  - `Standard`: balanced depth
  - `Thorough`: deeper risk scan
- Ask first intent question in that same prompt: `goal`+`audience`

RESOLVE missing or conflicting `Prompt 1` answers before intent completion. If request depends on existing repo or environment, inspect relevant context first when it would improve the options.

Intent completion normally happens after reasoning over `Prompt 1` and batches remaining intent questions as `Prompt 2`:
- `success criteria`
- `scope boundaries`+`non-goals`
- `hard constraints`

SPLIT intent completion when `Prompt 1` answer is broad, multi-audience, or high risk enough that `success criteria` should shape next questions:
- Prompt `2a`: `success criteria`
- Prompt `2b`: `scope boundaries`+`non-goals`+`hard constraints`

Depth Targets:
- `Quick` => `7-8`
- `Standard` => `12-13`
- `Thorough` => `17-18`

EXAMPLE: User picks `Quick`, plan for `7-8` prompts unless plan becomes complete sooner or needs more by prompt `8`.
- Prompt count tracks prompts, not raw question count. A batched prompt counts as `1` prompt.
- Count the opening prompts toward user selected depth target. If intent completion is split, `2a` and `2b` each count as one prompt.
- Treat `non-goals` as explicit exclusions that prevent nearby work from being pulled into plan.

## AFTER INTENT LOCKED

- Use the relevant `DOMAIN PACKS` as a hidden completeness frame.
- Ask next question based on what removes highest impact blocker.
- Prefer blocker severity over rigid checklist order.

## QUESTION RULES

- Reaching the lower end of the depth target never auto stops. Continue to selected depth target if high impact blocker remains.
- If question can be answered by exploring code or environment, explore over asking.
- Never ask low risk detail questions unless detail is likely to distort plan or implementation.
- If something is underspecified, ask unless assumption is low risk or unlikely to change implementation materially.
- Track resolved decisions, never repeat or lightly rephrase answered questions.
- Revisit resolved points only if new info creates inconsistency or invalidates earlier answer.
- Maintain a concise decision ledger after each answer:
  - `**Locked**`: explicit user decisions.
  - `**Assumed**`: low risk assumptions being carried forward.
  - `**Open**`: unresolved high impact blockers.
  - Keep the ledger short and only show it when it helps orient the next question or final plan.
- If user gives conflicting answers, stop and resolve contradiction directly before asking around it or continuing plan.
- Call out weak, inconsistent, or underspecified choices directly.
  - Force real tradeoff decision if weak assumption would materially change implementation.
  - Never press low impact disagreements if risk is understood and choice locked.
- Always add a fire 🔥 emoji instead of "(recommended)" directly after the user selectable option you most recommend for each question.

## DOMAIN PACKS

USE hidden domain-specific question packs only when relevant to the request. Don't walk every pack:
- `frontend`: user workflow, information architecture, state, accessibility, responsive behavior, empty/loading/error states.
- `backend`: API contracts, data model, persistence, permissions, failure modes, observability.
- `refactor`: behavior preservation, boundaries, migration path, test coverage, obsolete paths to remove.
- `migration`: source and target states, compatibility, rollout, rollback, data integrity.
- `debugging`: reproduction, expected behavior, observed behavior, likely fault boundary, verification.
- `AI feature`: inputs, outputs, evaluation, safety constraints, fallback behavior, latency/cost.
- `release`: rollout, monitoring, user communication, rollback, acceptance checks.

### EDGE CASES

ASK edge-case questions when handling them can materially change plan, implementation, UX, data integrity, rollout, or test strategy:
- Edge case prompts don't count toward selected depth target.
- Skip or stop once edge cases are irrelevant or can be handled by obvious defaults.
- Discover candidate edge cases through exploration, reasoning, and user selected answers before prompting. Examples: If a potential code change might introduce a new failure mode, explore code and use reasoning to identify what that failure mode might be before asking how to handle it. If user selects an answer that would introduce a known edge case, ask how to handle that edge case before proceeding.

## QUESTION STYLE

USE `request_user_input` for questions that materially change plan, confirm important assumptions, or choose between real tradeoffs:
- Default to one high impact question at a time.
- Ask `2-3` questions in one prompt if they're tightly coupled and batching can close branches faster than asking one at a time.
- Do not batch questions when one answer should shape the next question or when option combinations could conflict.
- Hard Exception: opening prompts must follow `OPENING SEQUENCE`.
- Offer `3-4` meaningful mutually exclusive options.

## CONTINUATION

ASK "Keep going?" with three options at the selected depth target:
- `Keep going`: continue asking as before
- `Stop here`: produce final plan
- `Three more`: ask three more, then ask "Keep going?" after that

## DONE

WHEN questioning done or user indicates completion, produce full handoff-ready `<proposed_plan>` complete on intent, locked decisions, non-goals, hard constraints, assumptions, implementation, and tests. Avoid leaving decisions to implementer.
