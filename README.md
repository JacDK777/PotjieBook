# PotjieBook

*In die pot, vanuit die hart.*

A South African potjiekos cookbook app, built with Claude.

## What it does
- Browse, search and filter recipes (Potjiekos, Potbrood, Braaivleis, Bykos, Poeding)
- Layer-by-layer "Pack the pot" diagram (moenie roer nie!)
- Tick-off method steps and keep-screen-on mode for cooking at the fire
- "Nou gaan ons Shop!" shopping list sorted by Checkers aisle, with Sixty60 search words
- Kiekie: add photos to any step or as general recipe photos
- Knows the pots: Falkirk No. 8B flat-bottom (default) and Falkirk No. 3 three-leg

## How it runs
`index.html` is the whole app (HTML, CSS and JavaScript in one file). It runs two ways:

**1. As a phone app (PWA).** Hosted on GitHub Pages, you open it in Chrome or Safari and install it to the home screen. It then:
- opens full-screen with its own icon, like any app
- keeps recipes and Kiekie photos on the phone (IndexedDB), so it works at the fire with no signal
- starts with the three recipes in `data/recipes/` the first time it opens
- has **Backup** at the bottom of the recipe list: *Save backup* downloads one file with every recipe and photo, and *Load backup or recipe* brings it back (or loads a single recipe file from `data/recipes/`)
- uses the phone browser's own camera, which fixes the Kiekie camera problem in the Claude app

**2. As a Claude artifact.** Inside Claude it uses the artifact runtime (`window.claude.use(...)`) as before: `db` for the `recipes` collection, `assets` for photos and `user` for edit rights.

The two keep separate books. To move recipes from the Claude version to the phone, export them to `data/recipes/` (as was done on 25 October 2026) and load the file with *Load backup or recipe*.

### Put it on your phone
1. On GitHub, open **Settings → Pages** and set **Source** to **GitHub Actions** (one time only).
2. Merge to `main`. The *Publish PotjieBook* workflow puts the app at `https://jacdk777.github.io/PotjieBook/`.
3. Open that link on the phone:
   - **Android (Chrome):** tap *Install PotjieBook* under Backup, or ⋮ → *Install app*
   - **iPhone (Safari):** Share → *Add to Home Screen*

When you change `index.html` or the recipes in `data/recipes/`, bump `VERSION` in `sw.js` so installed phones pick up the new files.

### Try it on a computer
```
python3 -m http.server 8000
```
Then open http://localhost:8000. Opening `index.html` straight from disk won't load the starter recipes, because browsers block that.

## What's in this repo
- `index.html`: the app
- `manifest.webmanifest`, `sw.js`, `icons/`: what makes it installable and work offline
- `.github/workflows/pages.yml`: publishes the app to GitHub Pages
- `recipes/`: readable copies of every recipe, including the house potjie *Bietjie Lam Groente*
- `data/recipes/`: the recipe data exactly as the app stores it (exported 25 October 2026). `index.json` lists the ones the phone app starts with.
- `docs/PROJECT_NOTES.md`: every decision, feature, pot size, known limit and the project history
- `docs/pots/`: photos of the Falkirk 8B and No. 3 pots

Live app (Claude): https://claude.ai/artifact/8kwkRckH34jfP8URVAcLBq
Phone app: https://jacdk777.github.io/PotjieBook/ (once Pages is switched on)
