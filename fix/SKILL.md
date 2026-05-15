---
name: fix
description: "Use proactively for technical fixes: bugs, failed tests, build failures, regressions, performance issues, integration failures, production issues, and unexpected behavior. Use root-cause investigation, reproduction, hypothesis testing, and narrow verification before changing code."
---
# Iron Rule
No fixes before root-cause investigation. If root cause is not understood, never propose or apply a fix. ALWAYS Gather evidence first.

## Workflow

### Phase 1: Investigate

Complete this before suggesting fixes:
- Read full errors, warnings, stack traces, file paths, line numbers, and error codes.
- Reproduce issue with exact steps, command, input, or failing test.
- Inspect recent changes, relevant config, dependencies, and environment differences.
- For multi-component failures, gather boundary evidence before proposing fixes: what enters each component, what exits each component, whether config, environment, state, and data propagate correctly.
- Trace bad values, state, or behavior backward until source found.

### Phase 2: Compare
Find the pattern before changing code:
- Locate similar working code or behavior in same codebase.
- Compare working and broken paths.
- Identify meaningful differences, including small config, data, dependency, and environment differences.

### Phase 3: Hypothesize
Use one hypothesis at a time:
- State suspected root cause clearly.
- Explain why the evidence supports it.
- Test the hypothesis with smallest possible probe or change.
- If it fails, discard hypothesis and return to evidence. Never stack fixes.

## Phase 4: Fix
Fix only after root cause identified:
- Create smallest useful failing reproduction first: automated test, command, script, or manual repro.
- Make one root-cause fix only.
- Avoid bundled refactors, opportunistic cleanup, and unrelated improvements.
- Verify the reproduction now passes.
- Run narrowest relevant validation for changed behavior.

If two fix attempts fail, stop. Discuss whether the architecture, pattern, or assumption is wrong before attempting another fix.

## Stop Signals
Return to investigation when you catch any of these:
- "quick fix"
- "try this"
- "probably"
- "one more fix"
- proposing fixes before tracing data flow
- changing multiple variables at once
- user pushback such as "stop guessing" or "is that happening?"

If investigation shows an environmental, timing, or external issue, document what was checked and what evidence supports that conclusion. Then add appropriate handling such as retry, timeout, clearer error, or diagnostic logging.
