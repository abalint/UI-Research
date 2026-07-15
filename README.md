# UI Research

A collection of research and reference material for reconstructing distinctive
software / game UI aesthetics on the web.

## Studies

### [`wowUI/`](./wowUI) — World of Warcraft (Classic) panel UI
Research on WoW Classic's **text / menu UI** — quest log, character sheet, options,
tooltips — rather than the gameplay HUD.

- **[`wowUI/RESEARCH.md`](./wowUI/RESEARCH.md)** — full design breakdown: the visual
  language, typography, color system, nine-slice frame construction, per-panel layouts,
  the reusable widget kit, and where to get exact texture assets.
- **[`wowUI/index.html`](./wowUI/index.html)** — a self-contained, viewable example
  page. A **pure-CSS** approximation of the panel kit (quest log, tooltip, options
  widgets, and the item-quality / class / text color palettes). Open it in any browser.

#### View the example
Open `wowUI/index.html` directly in a browser, or serve locally:

```sh
cd wowUI && python3 -m http.server 8000
# then visit http://localhost:8000
```
