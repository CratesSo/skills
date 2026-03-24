---
name: plan-my-grill
description: Interrogate a plan or design until it is handoff-ready. Start with intent, explore facts first, challenge weak choices directly, and prefer Plan mode clickable questions via request_user_input.
---

Interrogate the plan or design until it is handoff-ready and no high-impact blocker remains. Start with intent, then route questions by blocker severity instead of following a rigid checklist.

Always start by locking intent:
- Goal
- Success criteria
- Audience
- Scope boundaries
- Constraints

After intent is stable:
- Use a hidden completeness frame to track what still matters, such as interfaces, data flow, failure modes, tests, and rollout.
- Ask the next question that removes the highest-impact blocker.

Question selection rules:
- If a question can be answered by exploring the codebase or environment, explore instead of asking.
- Do not ask low-risk detail questions unless the missing detail is likely to distort the plan or implementation.
- If something is underspecified, ask unless the assumption is low-risk and unlikely to change the implementation materially.
- Track resolved decisions and do not repeat or lightly rephrase answered questions.
- Revisit a resolved point only if new information creates a real inconsistency or invalidates the earlier answer.
- Call out weak, inconsistent, or underspecified choices directly.
- Force a real tradeoff decision when a weak assumption would materially change the implementation.
- Do not keep pressing on low-impact disagreements once the risk is understood and the choice is locked.

When Plan mode is active:
- Use `request_user_input` for questions that materially change the plan, confirm important assumptions, or choose between real tradeoffs.
- Prefer the Codex app's clickable question UI instead of inline `a` / `b` / `c` answer menus.
- Default to one high-impact question at a time.
- You may ask 2-3 questions in one prompt only when they are tightly coupled and batching them will close the same branch faster with less wasted back-and-forth.
- Offer only meaningful, mutually exclusive options and make the recommended option clear.

When Plan mode is not active, or `request_user_input` is unavailable:
- Fall back to plain-text questions in chat.
- For each question, provide 2-5 concise example answers labeled with letters: `a`, `b`, `c`, etc.
- Mark the recommended answer clearly.
- Keep the options short enough that the user can reply compactly like `1a, 2c, 3b`.
- If none of the example answers fit, leave room for a freeform answer.

After 18 questions, ask "Should we keep going?" with three options, "Keep going", "Stop here", "Three more questions".
- If the user chooses "Keep going", continue asking questions as before.
- If the user chooses "Stop here", end the questioning and produce the final plan based on the information gathered so far.
- If the user chooses "Three more questions", ask three more questions before asking again.

Stop asking questions when no high-impact unresolved blocker remains or if the user chooses to stop.

When questioning is done, produce a full handoff-ready `<proposed_plan>` rather than loose summary. The plan must be complete on both intent and implementation, include tests and assumptions, and avoid leaving decisions to the implementer.
