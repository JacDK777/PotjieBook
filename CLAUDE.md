# PotjieBook

A South African potjiekos cookbook app, phone-first, in English with Afrikaans flair. Tagline: *In die pot, vanuit die hart.*

## Read first
- `project/HANDOVER.md`: what this is, where everything lives, open items
- `project/APP_REFERENCE.md`: data format, scaling syntax, how the code is organised
- `project/COOKING_PREFERENCES.md`: the user's pots and how they cook
- `project/RECIPES.md`: every recipe in full
- `docs/PROJECT_NOTES.md`: decision log and history

## Rules
- `index.html` is the whole app (HTML, CSS and JS in one file). It runs as a Claude artifact and as a PWA on GitHub Pages. Keep both paths working: the app talks to `window.claude.use("db" | "assets" | "user")`, and the IndexedDB stand-in provides the same shape outside Claude.
- Bump `VERSION` in `sw.js` whenever `index.html` or `data/recipes/` change.
- New shipped recipes: add the JSON to `data/recipes/`, add the id to `data/recipes/index.json`, and regenerate `recipes/<id>.md`.
- Keep the design: South African flag colour tokens, Alfa Slab One / Atkinson Hyperlegible / Courier Prime / Caveat, light and dark mode, phone width first.
- Don't add or change recipes unless the user asks. Recipes are drafted with the user first and saved only when they say they're ready.
- Test locally with `python3 -m http.server 8000` (opening the file from disk won't load the recipes).
- The live Claude artifact (https://claude.ai/artifact/8kwkRckH34jfP8URVAcLBq) isn't updated from this repo automatically. Say so when a change should also go there.
