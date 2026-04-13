# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**LNG 変態型人格測試量表** — A single-page personality quiz web app inspired by LNG livestreams. No matter what the user answers, the result is always `変態です / HENTAIDESU`.

## Files

| File | Purpose |
|---|---|
| `index.html` | Entire app — HTML, CSS, and JS all inline. No build step. |
| `question.json` | Source of truth for question content. |
| `hentaidesu.png` | Result screen image. Must stay in the same folder as `index.html`. |

## Running the app

Open `index.html` directly in a browser (`file://` works). No server, no dependencies, no install step.

## Adding or editing questions

1. Edit `question.json` to add/modify questions.
2. Copy the updated `questions` array into the `const QUESTIONS = [...]` in `index.html`. The JS reads from the inline constant, not from the JSON file, so both must be kept in sync.

Each question object requires: `id`, `category`, `question`, `options` (array of 4 strings). The `analysis` field in `question.json` is not used by the app.

## Architecture

`index.html` is a three-screen SPA with no framework:

- **Screens**: `#screen-intro`, `#screen-question`, `#screen-result`. Only one is visible at a time via CSS class `active`. `transitionTo(screenId)` handles the fade transition.
- **State**: A plain `state` object `{ questions[], current, answers[] }`. Questions are Fisher-Yates shuffled on each run; 5 are picked (currently the full pool of 5).
- **Result**: Always `変態です / HENTAIDESU` regardless of answers. The 4 fake MBTI dimension bars get randomized percentages (55–98%) each run; 2 comments are randomly picked from the `COMMENTS` pool of 8.
- **Particles**: A `<canvas>` RAF loop renders drifting pink/purple dots as the background. Entirely cosmetic.
- **Fonts**: Noto Serif JP loaded from Google Fonts. Falls back to Yu Gothic / serif when offline.

## Content pools (in `index.html`)

- `COMMENTS` — 8 nonsensical "analysis" strings; 2 are picked randomly per result.
- `DIMENSIONS` — 4 fake MBTI axes with labels and gradients (変態傾向, 腦洞深度, 中二指數, 羞恥心殘留).

To add more variety, extend `COMMENTS` or add new `DIMENSIONS` entries.

## Skills

`.claude/` is gitignored, so skills are local-only and must be reinstalled on each machine. Install from:
- `frontend-design`: https://github.com/anthropics/skills/tree/main/skills/frontend-design
- `webapp-testing`: https://github.com/anthropics/skills/tree/main/skills/webapp-testing

### `frontend-design`
Use when building or restyling any part of `index.html`. Commit to a bold aesthetic direction before coding — pick a tone (brutalist, maximalist, retro-futuristic, etc.) and execute it intentionally. Avoid generic choices: no Inter/Roboto/Arial, no purple-gradient-on-white, no cookie-cutter layouts. Use distinctive fonts (pair a display font with a body font), CSS variables for color consistency, purposeful animation, and atmospheric backgrounds.

### `webapp-testing`
Use when verifying UI behavior in `index.html`. Since the app is static HTML with no server, use the **static HTML path**: read the file directly to identify selectors, then write a Python Playwright script targeting `file://` URLs. Always `wait_for_load_state('networkidle')` before inspecting the DOM. Use `chromium.launch(headless=True)`.
