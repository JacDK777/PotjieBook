# PotjieBook project notes

Everything decided while building PotjieBook with Claude, so the work can carry on in Claude Code.

## The app
- **Name:** PotjieBook (one word)
- **Tagline and slogan:** *In die pot, vanuit die hart*
- **Language:** English, with Afrikaans flair (Alles, Bestanddele, Metode, Moenie roer nie!, Nuwe resep, Terug, Seker?, Die boek is leeg, Smaaklike ete!)
- **Live app:** https://claude.ai/artifact/8kwkRckH34jfP8URVAcLBq
- **Code:** `index.html` is the whole app. It runs as a Claude artifact and as an installable phone app (PWA) on GitHub Pages.

## Look and feel
- South African flag colours: black top bar, gold/green/white stripe under it, chilli red accents, flag blue filters, flag green for ticked steps
- Banner: the flag's green Y with the black and gold triangle, a red-to-gold Karoo sunset, windpomp, koppies and a potjie on the fire (dark mode: flag-blue night sky into red)
- Fonts: Alfa Slab One (headings), Atkinson Hyperlegible (body), Courier Prime (typed recipe-card details), Caveat (handwritten notes)
- Works in light and dark mode, built phone-first

## Features
- **Recipe book:** search by name or ingredient, filter by type (Potjiekos, Potbrood, Braaivleis, Bykos, Poeding, Ander), potjies listed first
- **Recipes:** pot, feeds, hours on the coals, heat, story/tips, ingredients by layer, method
- **Pack the pot:** pot diagram drawn from the layers, bottom to top. Layers named "On the side", "Side" or "Bykos" are side dishes and stay out of the diagram.
- **Cooking at the fire:** tick off steps, clear ticks, keep the screen on
- **Nou gaan ons Shop!:** shopping list from the ingredients, grouped by Checkers aisle (Slaghuis, Groente & vrugte, Suiwel & eiers, Bakkery, Drank, Spens). Tick off what's already in the kas, tap a word to copy it for the Sixty60 search, copy the whole list to buy.
- **Kiekies:** one floating Kiekie camera button per recipe, plus Load Kiekie for the gallery. Each photo is saved as a general photo or to any step. The first general photo becomes the cover. Tap a photo to view or remove it.
- **Kyk hoe lyk dit:** each step links to an image search showing what it should look like, plus "See dishes like this" for the finished dish

## Ingredient format in the editor
```
# Meat (brown first)
2 kg lamb potjie/stewing meat, bone-in
1 large onion, chopped

# Hard veg
750 g baby potatoes

# On the side
2 cups basmati rice
```
Lines starting with `#` start a new layer, bottom layer first. One ingredient per line. Method is one step per line.

## The pots
| Pot | Size | Feeds | Notes |
|---|---|---|---|
| Falkirk No. 8B flat-bottom (default) | about 35 × 13 cm, roughly 10–12 L (estimated from size, not measured) | 12–16 | Wide and shallow: great for potbrood and browning meat in one layer |
| Falkirk No. 3 three-leg | about 7.8 L (standard No. 3) | 8–10 | Deeper: classic layered potjiekos |

Photos: `docs/pots/`. To get an exact 8B size, fill it with a 1-litre jug to about 2 cm below the rim and count.

## Known limits
- **Sixty60:** Checkers has no public way for other apps to fill the Sixty60 trolley or check stock, so the app gives copyable search words instead.
- **Internet photos:** the app can't show photos from other websites (blocked in artifacts, and the photos belong to those sites). Hence the Kiekie photos and image-search links.
- **Kiekie camera:** the Claude app on Android blocks pages from opening the camera and the gallery, so Kiekie and Load Kiekie do nothing there, and no permission fixes it. The page now shows "Die kamera wil nie oopmaak nie" with the workaround. Use Chrome while signed in to claude.ai, the installed phone app, or send photos in a chat for Claude to upload.
- **Phone app storage:** recipes and photos live only on that phone. Clearing the browser's site data or uninstalling deletes them, so save a backup now and then. The app asks the browser to keep its data, but iPhones can still clear it if the app goes unused for weeks.
- **Two books:** the Claude artifact and the phone app don't sync with each other.

## Where the data lives
- **Claude artifact:** recipes live in the artifact's database (`recipes` collection), and photos in its asset store.
- **Phone app:** recipes and photos live in the phone's browser storage (IndexedDB database `potjiebook`, with stores `recipes`, `photos` and `meta`). Photo ids start with `local-`. It adds each recipe listed in `data/recipes/index.json` once (tracked in `meta.seededIds`).
- Neither is stored in this repo automatically. `data/recipes/*.json` is an export of the Claude recipes as of 14 November 2026. `recipes/*.md` are readable copies.
- A phone backup file is JSON: `{app:"PotjieBook", version:1, exportedAt, recipes:[{id, ...recipe}], photos:{id: dataURL}}`. *Load backup or recipe* also takes one recipe or a list of recipes.
- Changes in this repo don't update the Claude app. They reach the phone app when merged to `main` (bump `VERSION` in `sw.js`).

## Recipe data fields
`title, kind, pot, serves, hours, heat, notes, layers[{label, items[]}], steps[], photos[{id, url, step: number|"general", at}], yield{base, unit, options[]}, example, updatedAt`

`hours` with letters in it (like "10–12 min") is shown as written; a plain number gets "h" added.

## Scaling a recipe
A recipe with a `yield` gets a **Make [ ] sosaties** style dropdown (for example `{"base": 12, "unit": "sosaties", "options": [4, 6, 12, 24]}`). The choice is remembered per recipe on the phone.
- In ingredient lines, `{n}` marks an amount that scales: `{1.2} kg chicken`, `{½} English cucumber`.
- `[s]` after a word adds the ending only when the amount before it is more than 1: `{2} lemon[s]` shows "1 lemon" or "4 lemons".
- `{n} kg` under 1 kg is shown in grams. Amounts of 20 and up round to the nearest 5, 3 to 20 to the nearest ½, and small amounts to ¼, ⅓, ½, ⅔ or ¾.
- Feeds scales too when it's a plain number, and the shopping list uses the chosen size.
- Amounts without `{ }` never change. Method steps don't scale.

## How the house potjie was built: Bietjie Lam Groente

Started from Ananda Naude's Lamb and Mutton SA competition-winning lamb potjie, then changed together:
- Bacon removed, red wine cut to 125 ml (stock raised to 375 ml to compensate)
- Shanks swapped for 2 kg bone-in potjie/stew meat (about 2½ hours total)
- Mielies and green beans out; butternut, patty pans and baby marrows in, all chunky (soft veg goes in later)
- Knorr Brown Onion soup as thickener, sprinkled in dry a little at a time until thick enough
- 3 sprigs rosemary and 2 bay leaves with the liquid (removed before thickening), 30 g parsley at the end
- 2 cups basmati or jasmine rice on the side
- Later changes: 500 g mushrooms (two 250 g packs), one pack chopped and fried at the start, the other kept whole and added with the soft veg; 500 g butternut (1 small)
- Name: *Bietjie Lam Groente*, where "lam" means both lamb and lazy

Sources: [OFM / Lamb and Mutton SA](https://ofm.co.za/article/the-source/259559/lamb-and-mutton-sa-todays-winning-lamb-recipe), [Taste](https://taste.co.za/recipes/lamb-potjie/), [Daily Maverick](https://www.dailymaverick.co.za/article/2024-08-29-throwback-thursday-jerepigo-pinotage-lambs-neck-potjie.md), [Demand Africa](https://demandafrica.com/food/recipes/lamb-neck-potjie/)

## Greek Mint Chicken Sosaties
A braai recipe, built together from Drizzle and Dip, Daily Maverick, RecipeTin Eats and Rock Recipes:
- Chicken breast (not thighs), so the braai time is short: 10–12 minutes over medium-hot coals
- Yoghurt, olive oil, lemon, garlic, mint and oregano marinade, no chilli
- Double batch of marinade: half is set aside **before** the raw chicken goes in and used as the sauce, brushed on in the last 2–3 minutes and poured over to serve (gently warmed at most, never boiled, or the yoghurt splits)
- Red onion, green and red pepper and cherry tomatoes between the chicken
- No sides; mint tzatziki kept as an optional extra
- Base recipe makes 12 sosaties for 6 people; the app scales it to 4, 6, 12 or 24

Sources: [Drizzle and Dip](https://drizzleanddip.com/?p=11840), [Daily Maverick](https://www.dailymaverick.co.za/article/2025-10-20-deboned-chicken-thigh-sosaties.md), [RecipeTin Eats](https://www.recipetineats.com/chicken-souvlaki/), [Rock Recipes](https://www.rockrecipes.com/chicken-souvlaki-with-lemon-mint-tzatziki/)

## History
1. Cookbook app with recipes saved to the account, two example recipes
2. Karoo farmhouse theme with Afrikaans touches
3. Renamed PotjieBook; South African flag colours
4. Tagline and slogan: In die pot, vanuit die hart
5. Nou gaan ons Shop! shopping list
6. Pots added: Falkirk 8B (default) and Falkirk No. 3, with sizes
7. Bietjie Lam Groente created and added
8. Kiekie photos and image-search links; side dishes kept out of the pot diagram
9. Kiekie camera fix
10. Code and everything so far put in this repo
11. Phone app: installable PWA on GitHub Pages, works offline, recipes and photos kept on the phone, backup and restore
12. Bietjie Lam Groente: mushrooms split into two packs, 500 g butternut
13. Greek Mint Chicken Sosaties added, with the 4 / 6 / 12 / 24 sosaties dropdown that scales the ingredients and shopping list
14. Phone app: new recipes in the repo now reach phones where the app is already installed, without overwriting recipes changed or deleted on the phone
15. Kiekie: confirmed the Claude Android app blocks camera and gallery; added the "Die kamera wil nie oopmaak nie" message with workarounds
16. Handover pack in `project/` and `CLAUDE.md`, for setting PotjieBook up as a Claude Project or in Claude Code
