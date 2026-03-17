# Pareto-Analysis

A single-file, browser-based candidate evaluation tool that ranks options using a **thresholded dominance model** rather than simple averaging. No dependencies, no server, no install — open the HTML file in any browser.

---

## What It Does

You have 5 candidates (A through E) and 6 evaluation traits. You score each candidate 0–100 on each trait. The tool ranks them two ways:

- **Pareto Ranking** — thresholded dominance model (see below)
- **Average** — simple mean of all six trait scores

Both views update in real time as you edit scores.

---

## Traits

| Trait | Description |
|---|---|
| Charisma | Presence and interpersonal draw |
| Adaptability | Ability to adjust to changing conditions |
| Confidence | Self-assurance and decisiveness |
| Creativity | Novel thinking and problem-solving |
| Adventurousness | Willingness to take on risk and new challenges |
| Excitement | Energy and enthusiasm |

---

## Ranking Model

### Thresholded Dominance (Pareto Mode)

Standard Pareto ranking treats any difference as meaningful. This model adds a noise filter:

1. For each trait, compute the **standard deviation** across all 5 candidates
2. Set a **dominance threshold** = 0.5 × stddev for that trait
3. For each candidate pair A vs B, compare trait by trait:
   - If A − B ≥ threshold → **win** for A on that trait
   - If B − A ≥ threshold → **loss** for A on that trait
   - Otherwise → **neutral** (difference too small to matter)

**Strong dominance rule:** A strongly dominates B if A has at least 1 win, 0 losses, and wins > losses.

This prevents noise and minor score gaps from distorting the ranking.

### Tiering

| Tier | Criteria |
|---|---|
| Tier 1 | Zero strong losses, highest net dominance |
| Tier 2 | Mixed profile — some wins and losses, or no losses but limited wins |
| Tier 3 | Net dominance is negative (more losses than wins) |

Within each tier, candidates are sorted by net dominance → strong wins → category net score → fewer losses → alphabetical.

### Average Mode

Ranks candidates by simple mean of all 6 trait scores. Ties are broken alphabetically.

---

## Color System

### Column Badges (top of table)

Badge color reflects **average rank** in both modes — consistent regardless of which tab you're on:

| Color | Rank |
|---|---|
| 🟢 Dark green | 1st by average |
| 🟢 Green | 2nd |
| 🟡 Yellow | 3rd |
| 🟠 Orange | 4th |
| 🔴 Red | 5th |

In Pareto mode, the badge label shows Tier 1 / Tier 2 / Tier 3. In Average mode it shows 1st / 2nd / 3rd / 4th / 5th.

### Input Cell Shading

Per row, across all candidates:

| Color | Meaning |
|---|---|
| Green background | Highest score for that trait |
| Red background | Lowest score for that trait |
| Yellow background | Tie for highest or lowest |

### Radar Chart

Each candidate is plotted in their rank color. The chart uses a dynamic scale — the minimum ring value is set to the floor of the lowest score across all candidates, so differences between candidates that cluster in a narrow range are visually spread out.

---

## Features

- Live updates — all rankings, shading, and chart redraw on every keystroke
- Click any input to select the value and type over it
- **Randomize** button generates new scores biased around 75
- **Radar chart** — click to expand to a 75% screen popup with legend
- Explainer section describes the dominance model in plain language
- Fully self-contained — one HTML file, zero dependencies, works offline

---

## Usage

1. Download `pareto-ranking.html`
2. Open in any modern browser (Chrome, Safari, Firefox, Edge)
3. Enter scores for each candidate
4. Switch between Pareto Ranking and Average tabs to compare views
5. Click the radar chart to expand

No installation, no account, no internet connection required after download.

---

## File Structure

```
pareto-ranking.html   — entire application (HTML + CSS + JS, single file)
README.md             — this file
```

---

## Technical Notes

- Pure vanilla JS — no frameworks, no build step
- Canvas-based radar chart drawn with the HTML5 Canvas API
- Thresholded dominance computed fresh on every score change
- All state lives in a single `scores` object in memory
- Mobile-compatible layout with fixed-height header cells to prevent layout shift on tab switch

---

## License

MIT — use freely, modify as needed.
