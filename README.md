# PotjieBook

*In die pot, vanuit die hart.*

A South African potjiekos cookbook app, built with Claude.

## What it does
- Browse, search and filter recipes (Potjiekos, Potbrood, Braaivleis, Bykos, Poeding)
- Layer-by-layer "Pack the pot" diagram — moenie roer nie!
- Tick-off method steps and keep-screen-on mode for cooking at the fire
- "Nou gaan ons Shop!" shopping list sorted by Checkers aisle, with Sixty60 search words
- Kiekie: add photos to any step or as general recipe photos
- Knows the pots: Falkirk No. 8B flat-bottom (default) and Falkirk No. 3 three-leg

## How it runs
`index.html` is the whole app (HTML, CSS and JavaScript in one file). It is published as a Claude artifact and uses the artifact runtime (`window.claude.use(...)`) for:
- `db` — the `recipes` collection where recipes are stored
- `assets` — uploaded Kiekie photos
- `user` — checking who may edit

Opened outside Claude, the page loads but shows that recipes can't load here.

Live app: https://claude.ai/artifact/8kwkRckH34jfP8URVAcLBq
