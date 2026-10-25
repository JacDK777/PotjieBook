# PotjieBook project notes

Everything decided while building PotjieBook with Claude, so the work can carry on in Claude Code.

## The app
- **Name:** PotjieBook (one word)
- **Tagline and slogan:** *In die pot, vanuit die hart*
- **Language:** English, with Afrikaans flair (Alles, Bestanddele, Metode, Moenie roer nie!, Nuwe resep, Terug, Seker?, Die boek is leeg, Smaaklike ete!)
- **Live app:** https://claude.ai/artifact/8kwkRckH34jfP8URVAcLBq
- **Code:** `index.html` is the whole app. It runs as a Claude artifact.

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
- **Kiekie camera:** the first version used a hidden picker that some phones block. Now the buttons open the picker directly. Needs testing on the phone; if the Claude app won't open the camera, use PotjieBook in the phone's browser.

## Where the data lives
- Recipes live in the artifact's database (`recipes` collection), and photos in its asset store. Neither is stored in this repo automatically.
- `data/recipes/*.json` is an export of the recipes as of 25 October 2026. `recipes/*.md` are readable copies.
- Changes in this repo don't update the live app, and changes in the live app don't update this repo.

## Recipe data fields
`title, kind, pot, serves, hours, heat, notes, layers[{label, items[]}], steps[], photos[{id, url, step: number|"general", at}], example, updatedAt`

## How the house potjie was built: Bietjie Lam Groente
Started from Ananda Naude's Lamb and Mutton SA competition-winning lamb potjie, then changed together:
- Bacon removed, red wine cut to 125 ml (stock raised to 375 ml to compensate)
- Shanks swapped for 2 kg bone-in potjie/stew meat (about 2½ hours total)
- Mielies and green beans out; butternut, patty pans and baby marrows in, all chunky (soft veg goes in later)
- Knorr Brown Onion soup as thickener, sprinkled in dry a little at a time until thick enough
- 3 sprigs rosemary and 2 bay leaves with the liquid (removed before thickening), 30 g parsley at the end
- 2 cups basmati or jasmine rice on the side
- Name: *Bietjie Lam Groente*, where "lam" means both lamb and lazy

Sources: [OFM / Lamb and Mutton SA](https://ofm.co.za/article/the-source/259559/lamb-and-mutton-sa-todays-winning-lamb-recipe), [Taste](https://taste.co.za/recipes/lamb-potjie/), [Daily Maverick](https://www.dailymaverick.co.za/article/2024-08-29-throwback-thursday-jerepigo-pinotage-lambs-neck-potjie.md), [Demand Africa](https://demandafrica.com/food/recipes/lamb-neck-potjie/)

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
