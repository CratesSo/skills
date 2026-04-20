---
name: report
description: "Generate complete standalone HTML reports for explicit report requests, including recap and custom report modes. Use only when explicitly invoked as report."
---

# HTML RESPONSE

- Use only when user explicitly invokes `report`.
- Output only HTML. No markdown or prose before or after the doc.
- Always return complete HTML doc with `<!DOCTYPE html>`, `<html>`, `<head>`, and `<body>`.
- Never output HTML into chat.
- If user invokes `report` with text after it, treat that trailing text as the request.
- Reserve `report recap` and `report recap+` as the only special modes.
- If user invokes bare `report` with no trailing request, reply in chat with concise explanation of valid modes.

## MODE

- `report <user request>`
  - Format your next reply as a full HTML doc based on user request.
  - Treat any text after `report` as the request, including plain phrases like `break it down`.
  - Save it in workspace and open in Brave.
- `report recap`
  - Use most recent assistant reply in thread as only content source.
  - Reorganize into clean HTML doc without adding new substance.
  - Save in workspace and open in Brave.
- `report recap+`
  - Start from most recent assistant reply.
  - Add only concise, high value context already present in thread when it clearly improves the doc.
  - Good additions: omitted assumptions, caveats, tradeoffs, examples, or implementation notes already in scope.
  - Don't invent new facts, don't fetch outside info, and don't turn it into a transcript.
  - Save in workspace and open in Brave.
- If `recap` or `recap+` has no meaningful prior assistant reply in thread, fail clearly.

## HTML RULES

- Use compact `<style>` block in `<head>`.
- Keep styling self-contained and readable: typography, spacing, headings, lists, tables, code blocks, simple callouts.
- Prefer semantic HTML: `main`, `section`, `h1`-`h3`, `p`, `ul`/`ol`, `table`, `pre`, `code`, `blockquote`.
- Optimize for quick scanning, clear sections, minimal clutter, strong hierarchy.
- Match structure to task and keep it organized.
- If remote assets or external dependencies are needed, say so and await user confirmation before proceeding.
- Inline JavaScript is allowed when materially improving comprehension. Default to static HTML plus CSS.

## EXPORT WORKFLOW

- Treat current workspace root as active cwd for task.
- Save exports under `.html-response/` in workspace root.
- Filename format: `html-response-YYYYMMDD-HHMMSS.html`.
- Before replying, create the export folder if needed.
- Open the saved file in Brave with the normal `open` path first.
- If that fails, open file with Brave app path directly:
  - `open -a 'Brave Browser' <file>`
  - fallback: `'/Applications/Brave Browser.app/Contents/MacOS/Brave Browser' <file>`
