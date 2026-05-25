# Project Compass

A single-file, dark-themed decision-tree site that helps college students go from "I want to build something" to a finished, scoped project brief. Pick a field, narrow a focus, set a difficulty, optionally add research papers, and walk away with a tailored plan.

Built as one self-contained HTML file. No build step, no dependencies beyond Google Fonts. Open it in a browser.

## What it does

The site walks the user through five steps:

1. **Field** — Applied Math, Data Science, Computer Science, Biology, Engineering, or a "Build Your Own" path for anything else.
2. **Sub-area** — five focuses per field (25 total), or a guided custom narrowing for the BYO path.
3. **Difficulty** — Easy (1–2 weeks), Medium (3–6 weeks), or Hard (2+ months).
4. **Research papers** — opt in for three foundational reads per sub-area, or skip and just take the build plan.
5. **Brief** — a generated project brief with an overview, start steps, paper-reading tips, and a "how to know you're done" checklist.

The brief adapts to every selection. Easy projects get the basics. Medium adds information-gathering steps (YouTube curriculum, optional textbook). Hard adds those plus a full arXiv search walkthrough and reference-text guidance. The done-checklist gets stricter as difficulty rises.

## Build Your Own path

For students whose interests don't fit the five preset fields, the BYO path walks them through:

- Naming their field, with reflection prompts and a sanity check (Wikipedia, university departments, conferences)
- Narrowing to a sub-area, using subdiscipline lists, course catalogs, and conference section names
- The same difficulty and papers flow as the standard path
- A custom brief that uses their typed field/sub-area throughout
- If they opt into papers, a five-step guide on finding their own (surveys, citation tracking via Connected Papers and Semantic Scholar, arXiv strategy with fallbacks to JSTOR/PubMed/SSRN for non-arXiv fields)

## Running it

```bash
# Just open it in a browser.
open project-compass.html
# Or serve locally if you prefer:
python3 -m http.server 8000
# Then visit http://localhost:8000/project-compass.html
```

No build step. No `npm install`. Internet connection needed only for Google Fonts (Fraunces and JetBrains Mono).

## File structure

Everything is in `project-compass.html`:

- **CSS** — dark editorial theme using CSS custom properties. Background atmosphere via radial gradients and a subtle grid overlay. Animations are CSS-only.
- **HTML** — eight screens total (start, topic, custom-field, sub-area, custom-sub-area, difficulty, papers, brief), shown/hidden by a single active class.
- **JS** — data lives in a `DATA` object keyed by field. State is a single object tracking step, selections, and custom-path inputs. Navigation is by screen ID (numeric for the main flow, suffixed with `c` for custom).

## Customizing it

**Adding a field:** add a new key to the `DATA` object in the script tag. Each field needs a `title`, `desc`, and `subareas` object. Each sub-area needs a `title`, `desc`, and `papers` array (three entries with `title`, `authors`, `venue`, `url`).

**Changing the difficulty profiles:** edit `difficultyProfile()` to adjust durations, scope language, depth expectations, and ambition statements.

**Changing the start steps:** edit `startSteps()`. Steps can include inline HTML (`<strong>`, `<em>`, `<a>`) since the function renders via `innerHTML`. Medium and Hard get extra info-gathering steps inserted after the prior-art audit.

**Changing the done criteria:** edit `doneCriteria()`. The function returns base + medium + hard arrays that stack with difficulty level.

**Color/typography:** all colors live in the `:root` block as CSS variables. Fonts are loaded from Google Fonts in the `<link>` tag at the top.

## Tech notes

- Vanilla JS, no framework. Single file is easier to share, host, and tweak than a build pipeline for something this size.
- No persistent storage. State lives in memory only; refreshing resets the flow.
- Print-friendly: the "Save / print this brief" button on the summary screen uses `window.print()` and the layout holds up reasonably well as a printed page.
- Responsive down to ~360px. Difficulty grid and yes/no grid collapse to single column on narrow screens.

## Why it's built this way

A decision tree is fundamentally a state machine over screens. A single HTML file with a screen-id state and a few render functions is the simplest representation of that. React, a build step, or any framework would be more machinery than the problem needs. The trade-off is that the JS file gets long — but it's all in one place, which is easier to read and edit than the same logic spread across components, hooks, and a router.

The visual identity (Fraunces + JetBrains Mono, chartreuse accent on near-black, dashed dividers, italic section numbers) is meant to feel like a research notebook rather than a generic dark-mode app. Most "student project" tools default to bright, friendly UI; this one bets that students working on serious projects respond to something that takes itself seriously too.