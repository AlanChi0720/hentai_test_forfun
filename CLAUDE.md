# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**LNG 変態型人格測試量表** — A single-page personality quiz web app inspired by LNG livestreams. No matter what the user answers, the result is always `変態です / HENTAIDESU`.

## Files

| File | Purpose |
|---|---|
| `index.html` | Entire app — HTML, CSS, and JS all inline. No build step. |
| `questions.json` | Source of truth for question content. |
| `comments.json` | Source of truth for result-screen comments. |
| `dimensions.json` | Source of truth for fake MBTI dimension bars. |
| `pic_data/hentaidesu.png` | Result screen image. |
| `pic_data/mabushi_hentai.png` | Extra image asset. |

## Running the app

Open `index.html` directly in a browser (`file://` works). **No server, no Flask, no dependencies, no install step.** The JSON files are not loaded at runtime — they are human-editable source files only (see sync instructions below).

## Editing content

All three data sources follow the same pattern: edit the JSON file, then copy the updated array into the matching `const` in `index.html`.

### Questions (`questions.json` → `const QUESTIONS`)
1. Edit `questions.json`.
2. Copy the updated `questions` array into `const QUESTIONS = [...]` in `index.html`.

Each question object requires: `id`, `category`, `question`, `options` (array of 4 strings).

### Comments (`comments.json` → `const COMMENTS`)
1. Edit the `comments` array in `comments.json`.
2. Copy it into `const COMMENTS = [...]` in `index.html`.

2 comments are randomly picked per result run. Add more strings to increase variety.

### Dimensions (`dimensions.json` → `const DIMENSIONS`)
1. Edit the `dimensions` array in `dimensions.json`.
2. Copy it into `const DIMENSIONS = [...]` in `index.html`.

Each entry needs `label` (display text) and `gradient` (CSS `linear-gradient` string).

## Architecture

`index.html` is a three-screen SPA with no framework:

- **Screens**: `#screen-intro`, `#screen-question`, `#screen-result`. Only one is visible at a time via CSS class `active`. `transitionTo(screenId)` handles the fade transition.
- **State**: A plain `state` object `{ questions[], current, answers[] }`. Questions are Fisher-Yates shuffled on each run; 5 are picked from the pool of 15.
- **Result**: Always `変態です / HENTAIDESU` regardless of answers. The 4 fake MBTI dimension bars get randomized percentages (55–98%) each run; 2 comments are randomly picked from the `COMMENTS` pool.
- **Particles**: A `<canvas>` RAF loop renders drifting pink/purple dots as the background. Entirely cosmetic.
- **Fonts**: Noto Serif JP + Orbitron loaded from Google Fonts. Falls back to Yu Gothic / serif when offline.

## Testing

`test_app.py` — Playwright script that walks through all screens and asserts correct behaviour. Run with:
```
"C:/Users/alan8/AppData/Local/Programs/Python/Python311/python.exe" test_app.py
```
Screenshots are saved to `screenshots/`.

## Skills

`.claude/` is gitignored, so skills are local-only and must be reinstalled on each machine. Install from:
- `frontend-design`: https://github.com/anthropics/skills/tree/main/skills/frontend-design
- `webapp-testing`: https://github.com/anthropics/skills/tree/main/skills/webapp-testing

### `frontend-design`
Use when building or restyling any part of `index.html`. Commit to a bold aesthetic direction before coding — pick a tone (brutalist, maximalist, retro-futuristic, etc.) and execute it intentionally. Avoid generic choices: no Inter/Roboto/Arial, no purple-gradient-on-white, no cookie-cutter layouts. Use distinctive fonts (pair a display font with a body font), CSS variables for color consistency, purposeful animation, and atmospheric backgrounds.

### `webapp-testing`
Use when verifying UI behavior in `index.html`. Since the app is static HTML with no server, use the **static HTML path**: read the file directly to identify selectors, then write a Python Playwright script targeting `file://` URLs. Always `wait_for_load_state('networkidle')` before inspecting the DOM. Use `chromium.launch(headless=True)`.
