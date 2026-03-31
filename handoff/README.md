# handoff

- Summarize the active thread into a fresh-thread continuation prompt.

# PURPOSE

- Longer threads usually cost more per response because each new model call has to process more accumulated prior context before generating the next answer.
- This skill helps avoid bloated threads by making it easier and more effective to continue in a new thread.

## HOW TO USE

1. Invoke the skill manually when context is bloated to generate a handoff prompt for a new agent in a fresh thread.
2. Copy and paste the handoff block into a fresh thread.

## INSTALL

`npx skills@latest add CratesSo/skills/handoff`
