---
name: change-report
description: "Trigger: change report, visual report, screenshot report, reporte visual, capturas del change. Walk changed screens with Playwright and build an HTML report."
license: Apache-2.0
metadata:
  author: "Jorge Luis Bustamante Jara"
  version: "1.0"
---

## Activation Contract

Load when the user asks for a visual report of frontend changes made in the current session or change: every changed screen captured with Playwright and documented in one local HTML file.

## Hard Rules

- Requires Playwright MCP tools (`browser_navigate`, `browser_snapshot`, `browser_take_screenshot`; names may carry a runtime prefix such as `playwright_` or `mcp__playwright__`). If unavailable, STOP and say so; never fake screenshots.
- Document only changes that exist: derive them from session history, `git status`, `git diff`, and the files.
- Output is a LOCAL file. Never publish it as an Artifact or upload it anywhere.
- Write report text in the user's conversation language; keep paths, routes, and identifiers verbatim.
- Every image referenced in the HTML MUST exist on disk; verify before finishing.
- Do not edit application code or write any report file inside the repository.

## Decision Gates

| Situation | Action |
|-----------|--------|
| Dev server not reachable | Give the run command; start it only if the user confirms |
| Route needs login or seed data | Ask the user for credentials or steps once; never guess secrets |
| Change is a state (modal, error, hover, empty list) | Reproduce it with clicks/typing, then capture |
| Change is a specific element | Capture the full page AND an element shot (`ref`) |
| No visual surface (hook, service, API) | List it under the screen that consumes it, no image |
| Screenshot saved outside the report folder | Move it into `screenshots/` and fix the path |

## Execution Steps

1. Build the change list: files, affected routes, and what changed per screen, ordered by user flow.
2. Create the report folder OUTSIDE the project, in the user's home: `~/change-reports/{project-name}/{feature-slug}-{YYYYMMDD-HHmm}/screenshots/` (Windows: `%USERPROFILE%\change-reports\...`). Never write the report inside the repository.
3. For each screen: navigate, wait for content, reproduce the state, run `browser_snapshot` to find refs, then `browser_take_screenshot` with filename `NN-{screen}-{state}.png` inside `screenshots/`. Use a 1440x900 viewport; add a 390x844 shot when the change is responsive.
4. Copy `assets/report-template.html` to `index.html` in the report folder and fill one section per change: title, tag, route, description, files with `path:line`, images with captions.
5. Verify: every `<img src>` resolves to an existing file and no `{{placeholder}}` remains.
6. Give the user the absolute path of `index.html`.

## Output Contract

Return: report path, number of changes documented, number of screenshots, and every change that could not be captured with the reason.

## References

- `assets/report-template.html` — HTML template with placeholder blocks; fill rules are in its header comment.
