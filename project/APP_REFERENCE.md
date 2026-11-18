# PotjieBook app reference

How the app works and how to change it safely. For Claude in a new chat, a Claude Project or Claude Code.

## Two copies of the app
| | Claude artifact (main book) | Phone app (PWA) |
|---|---|---|
| Where | https://claude.ai/artifact/8kwkRckH34jfP8URVAcLBq | https://jacdk777.github.io/PotjieBook/ (from the repo via GitHub Pages) |
| Recipes | Artifact database, collection `recipes` | Phone browser storage (IndexedDB `potjiebook`: stores `recipes`, `photos`, `meta`) |
| Photos | Artifact asset store | On the phone (ids start with `local-`) |
| Code | Artifact page source | `index.html` in the repo |

They don't sync. Both copies share the same recipe screens. The repo's `index.html` adds a phone-app layer on top: an IndexedDB stand-in for the artifact's `db` and `assets`, backup and restore, and an install button. When you change the recipe screens, change both copies.

### The artifact
- Private to the owner. Capabilities declared: `db`, `user`, `assets`. Runtime contract 0.2.90.
- To change its code, read it with the Artifact tool's read action using the URL. Edit that HTML (it has no PWA layer), then publish to the same URL. Omit `capabilities` and `icon` to keep them. Don't publish the repo's `index.html` over it.
- Recipe and photo changes never need a republish. They go straight to the database (below).

### The repo (`JacDK777/PotjieBook`, private)
- `index.html`: the app (PWA version)
- `sw.js`: service worker. **Bump `VERSION`** (currently `potjiebook-v3`) whenever `index.html` or shipped recipes change, or installed phones keep the old files.
- `manifest.webmanifest`, `icons/`: what makes it installable
- `.github/workflows/pages.yml`: publishes to GitHub Pages on merge to `main`
- `data/recipes/*.json`: recipe exports. `index.json` lists those the phone app adds. Each id is added once per phone (tracked in `meta.seededIds`), so phone edits and deletes are kept.
- `recipes/*.md`: readable copies
- `docs/PROJECT_NOTES.md`: decision log and history
- `project/`: this handover pack. `CLAUDE.md` is for Claude Code.

## Recipe data (`recipes` collection)
Document id: a short slug, e.g. `lamb-potjie-chunky-veg`, `greek-mint-chicken-sosaties`.

| Field | Type | Notes |
|---|---|---|
| `title` | string | |
| `kind` | `Potjie` \| `Potbrood` \| `Braai` \| `Side` \| `Dessert` \| `Other` | Shown in Afrikaans: Potjiekos, Potbrood, Braaivleis, Bykos, Poeding, Ander |
| `pot` | string | `"Falkirk No. 8B flat-bottom"` (default), `"Falkirk No. 3 three-leg"`, `"No. 3"` style, or `""` for none. Exact Falkirk names show size info and a "jou pot" tag. |
| `serves` | string | `"6"` or `"6–8"`. A plain number scales with the size dropdown. |
| `hours` | string | `"2½"` shows as "2½ h". Text with letters, like `"10–12 min"`, shows as written. |
| `heat` | string | e.g. `"Low coals"`, `"Medium-hot coals"`, `"Coals under, coals on lid"` |
| `notes` | string | The story and tips, shown in a box under the title |
| `layers` | `[{label, items[]}]` | Bottom layer first. Labels starting "On the side", "Side" or "Bykos" are side dishes and stay out of the pot diagram. The diagram shows only for `kind: "Potjie"` with 2+ pot layers. |
| `steps` | string[] | One step each. Steps can be ticked off; ticks are stored per phone. |
| `photos` | `[{id, url, step, at}]` | `step` is a step index (0-based) or `"general"`. The first general photo is the cover. |
| `yield` | `{base, unit, options[]}` | Optional. Adds the size dropdown, e.g. `{"base":12,"unit":"sosaties","options":[4,6,12,24]}`. |
| `example` | boolean | `true` for the two placeholder recipes |
| `updatedAt` | number | Milliseconds timestamp |

### Scaling syntax (in ingredient lines)
- `{n}` marks an amount that scales: `{1.2} kg chicken`, `{½} English cucumber`, `Juice of {¼} lemon`.
- `[s]` after a word adds the ending only when the amount before it is more than 1: `{2} lemon[s]`, `({1} punnet[s])`.
- `{n} kg` under 1 kg shows in grams. Amounts of 20 and up round to the nearest 5, 3 to 20 to the nearest ½, and smaller amounts to ¼, ⅓, ½, ⅔ or ¾. Whole numbers stay exact.
- Amounts without `{ }` never change. Method steps don't scale. The shopping list uses the chosen size.

## Changing recipes from a Claude chat
Use the ArtifactData tool with the artifact URL:
- **Add:** `set` on collection `recipes` with a new `doc_id` and the full recipe. Include `"photos": []`.
- **Change:** `get` first, then `update` with only the changed fields and `if_version` set to the version you read. For a list field like `layers` or `steps`, send the whole list.
- **Rename:** `update` with `{"title": ...}`.
- **Delete:** `delete` with `if_version`, and only when the user asks.
- **Back up to the repo:** `list` the collection with `out_dir` set to the repo's `data/recipes`, regenerate `recipes/*.md`, add new ids to `data/recipes/index.json`, bump `sw.js` `VERSION`, and commit.

## Adding photos the user sends in chat
1. Upload each image to the artifact's asset store: Artifact publish with `url` set to the artifact URL, `file_path` set to the image, and `asset: true`. The result gives `id` and `url`.
2. `get` the recipe, then `update` its `photos` to the existing list plus `{"id", "url", "step": <0-based index or "general">, "at": <ms timestamp>}`, pinned with `if_version`.

## Features in the code (names to search for)
- Pots: `MY_POT` (8B), `MY_POT3`, `MY_POTS`, `POT_INFO` (size text), `POTS` (picker list)
- Types: `KINDS`, `KIND_AF`
- Shopping list: `renderShop`, `AISLES` (Slaghuis, Groente & vrugte, Suiwel & eiers, Bakkery, Drank, Spens), `aisleOf`, `searchTerm`. Ticks per phone in `have:<id>`.
- Scaling: `scaleText`, `fmtNum`, `parseNum`, `yieldCount`, `scaleOf`. Choice per phone in `yield:<id>`.
- Photos: `onPicked`, `openSheet`, `openPhoto`, `pickerWatch` (shows "Die kamera wil nie oopmaak nie" when no picker opens), `shrink` (resizes to 1600 px JPEG)
- Image-search links: `lookQuery`, `IMG_SEARCH`
- Step ticks per phone in `done:<id>`

## Known limits
- **Camera and gallery in the Claude Android app** are blocked by the app, so Kiekie and Load Kiekie can't open there. Use Chrome while signed in to claude.ai, the phone app, or send photos in chat.
- **Checkers Sixty60** has no public way for other apps to fill the trolley or check stock. The app gives copyable search words.
- **Internet photos** can't be shown in the artifact (blocked, and they belong to those sites), hence the image-search links.
- **Pot sizes:** the 8B size is estimated from 35 × 13 cm, not measured.
