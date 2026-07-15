# The WoW Classic Panel UI — Design Reference

Research on the World of Warcraft (Classic) **text / menu UI** — the panel system
(quest log, character sheet, options, tooltips) rather than the gameplay HUD — with
everything needed to reconstruct it on the web.

---

## 1. The core visual language

Classic WoW's non-combat UI is a **faux-medieval "fantasy stone-and-gold"** system.
Almost every window is built from the same kit, which is why it feels so cohesive.
Recurring motifs:

- **Heavy iron/gunmetal frames** — the main windows (quest log, character, etc.) are
  bordered by a thick **warm gunmetal band** (dark brownish-grey brushed metal, *not*
  gold), with a beveled highlight along the inner edge and a visible surface **grain
  texture**. Gold appears only as accents — the portrait ring, the title text, button
  labels — not as the frame itself.
- **A circular portrait medallion** inset into the **top-left** of most major windows
  (character sheet, quest log, spellbook, talents), set in an ornate gold ring.
- **A title** centered along the top in an antique serif.
- **A round red "X" close button** in the top-right.
- **Tabs along the bottom edge** for switching sub-views, drawn as notched trapezoids.
- **Two interior surface types:** dark semi-transparent stone (options, lists,
  tooltips) and **tan parchment** (quest text, letters, books).
- Everything is **nine-slice**: a tiling center + stretchable edges + fixed decorative
  corners. This is the single most important concept for reproducing it faithfully.

The overall mood is dim, warm, and weighty — dark backgrounds, gold/amber accents,
warm off-white text. Nothing is flat; every surface has a bevel, gradient, or texture.

---

## 2. Typography

| Role | Font | Where it's used |
|---|---|---|
| **Headers / titles** | **Morpheus** | Window titles, quest titles, display text. Tall, branching, calligraphic serif. |
| **Body / labels / tooltips** | **Friz Quadrata TT** | The workhorse — item names, tooltips, most labels, quest body, button text. Glyphic serif with flared wedge serifs. |
| **Numbers / chat** | **Arial Narrow** | Chat window, numeric readouts, condensed data. |
| **Combat text** | Skurri | (HUD only — out of scope.) |

**Web substitutes** (real fonts are licensed / not web-safe):

- Morpheus → **Cinzel Decorative** / **MedievalSharp** for titles.
- Friz Quadrata → **Marcellus**, **Cormorant**, **Caudex**, or **Vollkorn**; Georgia as
  a system fallback.
- Arial Narrow is web-safe; **Roboto Condensed** is a good free match.

Text almost always has a subtle **dark drop-shadow** (offset ~1px, black) — baked into
the WoW look. CSS: `text-shadow: 1px 1px 0 rgba(0,0,0,0.9);`

---

## 3. The color system

### Text colors (semantic)
- **Heading gold/yellow** `#FFD100` (RGB 1.0, 0.82, 0) — section headers, interactive labels.
- **Normal white** `#FFFFFF` — standard body text.
- **Green** `#1EFF00` — usable/available, quest objectives complete.
- **Red** `#FF2020` — errors, level-too-high, unusable.
- **Grey** `#808080` / `#9D9D9D` — disabled, unavailable, flavor text.

### Item quality colors (loot rarity)
| Quality | Hex |
|---|---|
| Poor (grey) | `#9D9D9D` |
| Common (white) | `#FFFFFF` |
| Uncommon (green) | `#1EFF00` |
| Rare (blue) | `#0070DD` |
| Epic (purple) | `#A335EE` |
| Legendary (orange) | `#FF8000` |
| Artifact / Heirloom | `#E6CC80` |

Used to color item names in tooltips **and** to tint the tooltip's border.

### Class colors (Classic-era)
| Class | Hex | Class | Hex |
|---|---|---|---|
| Warrior | `#C69B6D` | Rogue | `#FFF468` |
| Paladin | `#F48CBA` | Priest | `#FFFFFF` |
| Hunter | `#AAD372` | Shaman | `#0070DD` |
| Mage | `#3FC7EB` (old `#69CCF0`) | Warlock | `#8788EE` |
| Druid | `#FF7C0A` | Death Knight | `#C41E3A` |

### Surface colors
- **Dark interior / tooltip bg:** near-black, faint cool tint, ~`rgba(10,10,16,0.9)`.
- **Parchment:** warm tan, ~`#C8B48C` → `#B29155` gradient, ideally with paper noise.
- **Frame gold/bronze:** `#4A3B22` (dark) → `#C7A857` (highlight).

---

## 4. How the frames are built (nine-slice)

Every panel is a **Backdrop**: a `bgFile` (tiled interior) + an `edgeFile` (border
sliced into 4 corners + 4 edges) + `insets` (how far the bg sits inside the border).
Two standard kits dominate:

- **A. Tooltip / thin kit** — `Interface/Tooltips/UI-Tooltip-Border` +
  `UI-Tooltip-Background`. Thin (~16px) gold-brown border, dark interior.
- **B. Dialog / heavy kit** — `Interface/DialogFrame/UI-DialogBox-Border` +
  `UI-DialogBox-Background`. Thick ornate carved border, dark stone interior.

Big "book" windows (Character, Quest Log, Spellbook, Talents) use **bespoke full-frame
art**: one large texture with the portrait ring, title, and border painted in, plus a
parchment or list area beneath.

### Translating nine-slice to CSS
1. **`border-image`** with a sliced PNG (most faithful):
   ```css
   .wow-panel {
     border: 24px solid transparent;
     border-image: url(border.png) 24 fill repeat;
   }
   ```
2. **Layered box-shadow + gradients** (texture-free approximation):
   ```css
   .wow-panel {
     background: rgba(10,10,16,0.92);
     border: 2px solid #1a1a1a;
     box-shadow:
       inset 0 0 0 2px #402d10,
       inset 0 0 0 3px #c7a857,
       inset 0 0 0 5px #402d10,
       0 0 12px rgba(0,0,0,0.8);
   }
   ```

---

## 5. The specific panels

### Quest Log
- Two-region layout. **Left: quest list** — quests under **collapsible zone headers**
  (`+`/`−`). Each entry `[level] Quest Name`, the **level tinted by difficulty**
  (red/orange/yellow/green/grey vs your level). **Right: quest detail** on **parchment** —
  title in gold, then Objectives, Description, Rewards (reward items as quality-bordered
  icon buttons). Scrollbar per region. Bottom: Abandon Quest.

### Character panel
- **Portrait medallion top-left** (3D bust in the ring). **Equipment slots** in two
  columns flanking the model; equipped items get a **quality-colored border**. **Stats**
  as label/value rows. **Bottom tabs**: Character / Reputation / Skills, as notched
  trapezoids; active tab raised/brighter.

### Options / Settings
- **Dark dialog-kit panel** (kit B), not parchment. **Left category list** or top tabs.
  Right pane of **checkboxes, sliders, dropdowns, gold section headers**. **Okay /
  Cancel / Defaults** gold beveled buttons along the bottom.

### Tooltips
- Small dark panel (kit A). Item name line **colored by quality**, **border tinted to
  match**. Below: type, white stats, red "requires level", gold italic flavor text,
  coin sell price. Pattern to copy: **dark translucent bg + quality-colored border +
  gold/white/red text hierarchy.**

---

## 6. The reusable widget kit

Build once, reuse everywhere:

- **Buttons** — two distinct styles:
  - **Standard** (`UIPanelButtonTemplate`): stone/metal pill, raised gold bevel, gold
    label, **glow on hover**, **inset pushed state**. Disabled = grey label.
  - **Red action buttons** (`MagicButton` style): the bottom action bar of the quest
    log / character frame uses **deep-red gradient buttons** (bright red top → dark
    maroon bottom, ~`#a02722`→`#4e0c0a`) with a top bevel highlight and **gold/yellow
    text** (`#ffd100`). Disabled buttons go flat dark-grey with grey text. This is the
    look in the reference screenshot (Abandon / Share / Track / Options / Close).
- **Close button**: round **red X circle**, top-right, overlapping the edge.
- **Checkboxes**: square metal frame; checked = **gold check**.
- **Sliders**: engraved groove + **gold knob**; label + min/max/value.
- **Dropdowns** (`UIDropDownMenu`): recessed bar with decorative end-caps, down-arrow,
  dark popup list.
- **Scrollbars**: ornate up/down arrow buttons + draggable thumb on an engraved track.
- **Tabs**: **notched trapezoid**; active brighter/raised.
- **Money display**: gold/silver/copper counts with coin icons.
- **Icon wells**: beveled square recesses (`64×64` art shown ~`37px`), optional quality
  border + cooldown sweep.

---

## 7. Layout & interaction conventions

- **Fixed-size panels**, not fluid — pieces of art at a set resolution. Pick a base size
  and scale proportionally; don't let content reflow the frame art.
- Windows are **draggable by the title region** and **stack** when multiple open.
- **No flat cards, thin hairlines, or modern spacing.** Density is high, padding tight,
  every container visibly framed.
- **Warm dark palette + gold accents + serif text + drop-shadowed text** are the four
  things that instantly read as "WoW."

---

## 8. Best source material for exact assets

- **`Gethe/wow-ui-textures`** (GitHub) — mirror of the actual UI texture PNGs (tooltip
  borders, dialog frames, buttons, parchment, portrait rings). Use with `border-image`.
- **`tekkub/wow-ui-source`** (GitHub) — Blizzard's own UI XML/Lua;
  `SharedXML/SharedUIPanelTemplates.xml` and `FrameXML/*` show exact templates, insets,
  edge sizes, anchors.
- Data tables: [Class colors](https://warcraft.wiki.gg/wiki/Class_colors) ·
  [Quality](https://warcraft.wiki.gg/wiki/Quality) ·
  [XML/Backdrop](https://wowpedia.fandom.com/wiki/XML/Backdrop) ·
  [Quest Log](https://vanilla-wow-archive.fandom.com/wiki/Quest_Log)

---

## Sources

- [What Font Does Warcraft Use (Designyourway)](https://www.designyourway.net/blog/warcraft-fonts/)
- [WoW Font (MadeGoodDesigns)](https://madegooddesigns.com/world-of-warcraft-font/)
- [Quality — Warcraft Wiki](https://warcraft.wiki.gg/wiki/Quality)
- [Class colors — Warcraft Wiki](https://warcraft.wiki.gg/wiki/Class_colors)
- [XML/Backdrop — Wowpedia](https://wowpedia.fandom.com/wiki/XML/Backdrop)
- [Quest Log — Vanilla WoW Wiki](https://vanilla-wow-archive.fandom.com/wiki/Quest_Log)
- [wow-ui-textures (GitHub)](https://github.com/Gethe/wow-ui-textures)
- [wow-ui-source (GitHub)](https://github.com/tekkub/wow-ui-source)
