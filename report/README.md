# report
- Generates and opens a saved HTML report as a response format.
  - Exports the document into the workspace, then opens it in `Brave`.

## How to use
1. **Edit Skill** to use your browser of choice.
2. Type `$report <request>` or use `report recap` / `report recap+`.
3. Follow the HTML export flow for the selected mode.

### Examples:
- `$report Show me the performance of the latest simulation.`

- `$report recap`
  - Generates an HTML report of the **LAST** assistant reply in thread using only content source.
  
- `$report recap+`
  - Generates an HTML report from the last assistant reply along with any high value context already present in thread when it clearly improves the doc.

## Install
`npx skills@latest add CratesSo/skills/report`
