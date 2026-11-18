# PotjieBook handover

*In die pot, vanuit die hart.*

Everything needed to pick up PotjieBook in a new Claude chat, a Claude Project or Claude Code. Written 18 November 2026.

## 1. What PotjieBook is

A personal South African cookbook app, mostly for potjiekos, built with Claude for the user's own family cooking. It's phone-first and made to be used at the fire.

- **English with Afrikaans flair:** Alles, Bestanddele, Metode, Moenie roer nie!, Nuwe resep, Terug, Seker?, Smaaklike ete!
- **South African flag colours:** black top bar, gold/green/white stripe, chilli red, flag blue and flag green, with a Karoo sunset banner (windpomp, koppies, a potjie on the fire)
- **What it does:**
  - search and filter recipes
  - draws a "Pack the pot" layer diagram
  - lets you tick off steps at the fire and keeps the screen on
  - builds a "Nou gaan ons Shop!" shopping list sorted by Checkers aisle, with Sixty60 search words
  - adds photos ("Kiekies") to any step
  - links each step to an image search ("Kyk hoe lyk dit")
  - scales recipes with a size dropdown (sosaties: 4 / 6 / 12 / 24)

## 2. Where everything lives

| What | Where | Notes |
|---|---|---|
| Live app (the main recipe book) | Claude artifact: https://claude.ai/artifact/8kwkRckH34jfP8URVAcLBq | Private to the owner. Recipes are in its database, photos in its asset store. |
| Code, docs and recipe backups | GitHub: `JacDK777/PotjieBook` (private) | `index.html` is the app. `project/` holds this handover pack. |
| Installable phone app (PWA) | GitHub Pages: https://jacdk777.github.io/PotjieBook/ | Built by a Claude Code session. Keeps its own separate book on the phone. Works only once Pages is switched on (see section 5). |

The Claude artifact and the phone app **do not sync**. The Claude artifact is the main book.

## 3. Current state (18 November 2026)

**Recipes in the book**
1. **Bietjie Lam Groente**: the house lamb potjie. Falkirk 8B, feeds 6–8, about 2½ h. "Lam" means both lamb and lazy.
2. **Greek Mint Chicken Sosaties**: braai, 12 sosaties for 6 people, with a 4 / 6 / 12 / 24 size dropdown. Half the marinade is kept back as the sauce.
3. **Oxtail Potjie with Red Wine**: an example recipe from the first build.
4. **Simple Potbrood**: an example recipe from the first build.

Full recipes are in `RECIPES.md`.

**Photos:** none yet. The asset store is empty.

**The pots**

| Pot | Notes |
|---|---|
| **Falkirk No. 8B flat-bottom** | The default pot. About 35 × 13 cm, roughly 10–12 L, feeds 12–16. The size is estimated, not measured. |
| **Falkirk No. 3 three-leg** | About 7.8 L, feeds 8–10. |

## 4. How the work has been done (keep doing it this way)

1. **Recipes are built together before they're saved.** Claude researches good versions online and drafts one in chat, the user changes it step by step, and Claude shows the full recipe for checking when asked. **Nothing goes into PotjieBook until the user says it's ready.** Once, Claude added a recipe too early and had to take it out; the user was clear about this.
2. **Changes to saved recipes** are made directly in the app's database, keeping everything else as it is.
3. **After a meaningful change,** the GitHub repo is updated too (export recipes, regenerate `recipes/*.md`, update `docs/PROJECT_NOTES.md`). This happens only when the user asks or agrees.
4. **Answers go in short, plain language,** not technical detail, unless the user asks.

## 5. Open items and next steps

| # | Item | Status |
|---|---|---|
| 1 | **Kiekie in the Claude app on Android does nothing.** The Claude Android app blocks pages from opening the camera and gallery. There's no permission that fixes it. | Workarounds: open PotjieBook in Chrome while signed in to claude.ai, or send photos in a chat and Claude uploads them (see `APP_REFERENCE.md`). The page now shows a "Die kamera wil nie oopmaak nie" message. Not yet confirmed whether it works in Chrome. |
| 2 | **Measure the Falkirk 8B.** Fill it with a 1-litre jug to about 2 cm below the rim and count. | Waiting on the user. Then update `POT_INFO` in the app. |
| 3 | **Example recipes** (oxtail, potbrood): switch them to the user's pots, or delete them? | Asked, never answered. |
| 4 | **Size dropdown for Bietjie Lam Groente?** | Offered, never answered. |
| 5 | **GitHub Pages** must be switched on once (repo Settings → Pages → Source: GitHub Actions) before the phone app link works. | Unknown whether done. |
| 6 | **Phone app installed before 14 November** keeps its old copy of Bietjie Lam Groente (without the mushroom and butternut changes). | Edit it on the phone, or delete it there so the repo version is added again. |
| 7 | **Claude memory is off** for this account, so a new chat won't know any of this unless these files are in a Project or pasted in. | Hence this handover pack. |

## 6. Setting it up as a project

### Option A: a Claude Project (for cooking and recipe chats)
1. In Claude, create a new project called **PotjieBook**.
2. Paste the text of `PROJECT_INSTRUCTIONS.md` into the project's instructions.
3. Add these files to the project's knowledge:
   - `HANDOVER.md` (this file)
   - `RECIPES.md`
   - `COOKING_PREFERENCES.md`
   - `APP_REFERENCE.md`
4. Optionally add the two pot photos from `docs/pots/`.
5. Start chats about new recipes or app changes inside that project.

### Option B: Claude Code (for changing the app's code)
1. Open Claude Code and pick the `JacDK777/PotjieBook` repository.
2. `CLAUDE.md` at the top of the repo loads automatically and points to everything else.

Both can be used side by side.

## 7. Files in this pack

| File | What it's for |
|---|---|
| `HANDOVER.md` | This overview |
| `PROJECT_INSTRUCTIONS.md` | Paste into a Claude Project's instructions |
| `RECIPES.md` | Every recipe in full |
| `COOKING_PREFERENCES.md` | Pots, how the user cooks, likes and dislikes, how recipes get made |
| `APP_REFERENCE.md` | How the app works and how to change recipes, photos and code safely |
| `../CLAUDE.md` | Picked up automatically by Claude Code |
| `../docs/PROJECT_NOTES.md` | Full decision log and history |
