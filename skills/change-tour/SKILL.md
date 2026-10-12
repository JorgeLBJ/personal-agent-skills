---
name: change-tour
description: "Trigger: change tour, recorrido, guided tour, walkthrough of session changes, show me what you built. Present frontend changes stop by stop, waiting for confirmation."
license: Apache-2.0
metadata:
  author: "Jorge Luis Bustamante Jara"
  version: "1.0"
---

## Activation Contract

Load when the user asks for a guided tour of changes made in the current session (by you or a delegated agent), especially changes that add or edit frontend UI.

## Hard Rules

- Present ONE stop per turn. After each stop, STOP and wait for the user's confirmation. Never chain stops.
- Every stop MUST start with the header from `assets/stop-header.md`, filled with the current stop number, total stops, and a progress bar.
- Announce the total number of stops in the first message and keep it stable; if it must change, say why in the header note.
- Only describe changes that actually exist: verify against the session history, `git diff`/`git status`, or the files. Never invent features.
- Speak to the user in their conversation language; keep file paths, identifiers, and routes verbatim.
- Do not edit code during the tour. If the user asks for a fix, note it and continue (or exit) only when they say so.

## Decision Gates

| Situation | Action |
|-----------|--------|
| Playwright MCP tools available (`browser_navigate`, `browser_take_screenshot`, any runtime prefix) and dev server reachable | Navigate to the route, take a screenshot, highlight the changed element (snapshot/hover) |
| Playwright available but server not running | Give the exact run command and URL; ask the user to start it or confirm you may start it |
| No Playwright | Give the full URL/route and the exact UI steps (click X → open Y) to see the change |
| Change has no visual surface (hook, service, API) | Group it into the stop of the UI that consumes it; list files with `path:line` |
| User says "skip", "next", "siguiente", "ok", "dale" | Advance to the next stop |
| User asks a question | Answer it, then re-show the "waiting" footer without advancing |

## Execution Steps

1. Collect the change set: session history, `git status`, `git diff --stat`, and relevant diffs.
2. Group changes into stops by user-visible flow (screen/route/component), ordered as the user would navigate. Backend/support code attaches to the stop that uses it.
3. Send the tour index: total stops and a one-line title per stop. Wait for confirmation to start.
4. For each stop: render the header, then list what was created/changed (`x`, `y`, `z`), files with `path:line`, how to see it (Playwright screenshot or route + steps), and what to verify.
5. End each stop with the waiting footer from `assets/stop-header.md`, then STOP.
6. After the last stop, render the closing summary block from `assets/stop-header.md`.

## Output Contract

Per stop: header → "What was built" bullets → "Files" list → "How to see it" (screenshot or route/steps) → "Check" bullets → waiting footer. Final turn: closing summary with all stops marked done and any notes/fixes the user raised.

## References

- `assets/stop-header.md` — header, progress bar, waiting footer, and closing summary templates.
