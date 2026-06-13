# THD Advanced Analytics — Deck Design Language

A portable spec for the **"Intelligence at Scale"** presentation aesthetic: a full-viewport HTML slide deck in a clean, technical, paper-flat style with a single Home Depot orange accent. Copy this file into a sibling deck and an agent can rebuild the same look without seeing the original.

The feel: a **serious analytics instrument**, not a marketing landing page. White paper, 1px hairline borders, mono tabular numbers, uppercase micro-labels, restrained entrance motion. Depth comes from hairlines — not heavy shadows, not glass, not gradients (except one 3px brand strip). Every slide fills the viewport between a fixed brand bar and status bar.

---

## 1. Core Principles

1. **Light, paper-flat surfaces.** Background is pure white. Chrome (brand bar, status bar, card headers, table headers) is a barely-off-white (`--bg-chrome` / `--bg-stripe` `#FAFAFA`). Structure comes from 1px hairline borders. The only large shadow is on the floating nav pill.
2. **Hairlines, not boxes.** Every division is a 1px border in one of three grays. Cards/panels = 1px border + small radius (6px). Never use heavy borders or drop shadows for in-page structure.
3. **Tabular numbers.** All numbers, metrics, counts, percentages use `.mono` → `font-variant-numeric: tabular-nums`. Display numbers are heavy weight (700) with slight negative tracking.
4. **Uppercase micro-labels.** Eyebrows, card headers, stat labels, field labels, status-bar text: 10–11px, uppercase, `letter-spacing: .08em`, color `--ink-3`. This is the single most identifying texture of the system.
5. **One accent — THD orange.** `#F96302`. Marks the active state, the primary button, focus, the eyebrow bar, the active nav dot, selection tints. **Never** a large background wash. Semantic colors (ok/warn/err) are status only — `--ok` green also tags "in production / proven."
6. **Restraint over decoration.** Generous but not airy. Type runs 10–19px. No friendly oversized radii, no gradients beyond the brand strip, no glassmorphism.
7. **NO left-edge color rails on cards.** Hard rule. Use a tinted header strip (`--accent-tint`), a soft border (`--accent-soft`), or a leading accent dot — never a vertical accent bar down the side of a card.

---

## 2. Design Tokens

Drop this `:root` block in verbatim. Load **Manrope** (400–800).

```css
:root{
  --font-sans:"Manrope",-apple-system,BlinkMacSystemFont,"Segoe UI",Tahoma,Arial,sans-serif;
  --font-mono:"Manrope",-apple-system,BlinkMacSystemFont,"Segoe UI",Tahoma,Arial,sans-serif;

  /* Surfaces — clean light palette */
  --bg-app:#FFFFFF; --bg-chrome:#FAFAFA; --bg-paper:#FFFFFF; --bg-input:#FFFFFF;
  --bg-hover:#F4F4F4; --bg-stripe:#FAFAFA;

  /* Text ramp, darkest → lightest */
  --ink:#111111; --ink-2:#3F3F3F; --ink-3:#757575; --ink-4:#A8A8A8;

  /* Borders, lightest → strongest */
  --line:#E5E5E5; --line-2:#EFEFEF; --line-strong:#D0D0D0;

  /* Accent — THE HOME DEPOT ORANGE (the only accent) */
  --accent:#F96302; --accent-ink:#C24E02; --accent-soft:#FBD9C0; --accent-tint:#FFF3EA;

  /* Semantic — status only, never decoration. --ok also = "proven / in production" */
  --ok:oklch(0.55 0.12 150); --ok-soft:oklch(0.94 0.05 150);
  --warn:oklch(0.62 0.13 70); --warn-soft:oklch(0.95 0.06 75); --warn-ink:oklch(0.48 0.13 65);
  --err:oklch(0.55 0.18 25);

  /* The ONLY large shadow — for the floating nav pill / popovers */
  --shadow-window:0 1px 0 rgba(0,0,0,.03),0 12px 32px -8px rgba(60,40,20,.18),0 2px 6px rgba(60,40,20,.08);

  /* Radii — small */
  --r-card:6px; --r-input:4px; --r-win:10px;
}
```

Global body: `font-size:14px; line-height:1.45; letter-spacing:-.005em;` with `font-feature-settings:"tnum" 1,"ss01" 1;` and `-webkit-font-smoothing:antialiased`. `body{overflow:hidden}` — the deck never scrolls; each slide fits the viewport.

### Brand strip (the only gradient allowed)
A 3px horizontal hairline directly under the brand bar — orange with a small cadmium-yellow tail. Use **only** here (or the top of a modal). Never as a fill.

```css
.brandstrip{ height:3px;
  background:linear-gradient(to right,#F96302 0,#F96302 84%,#F5A623 84%,#F5A623 100%); }
```

---

## 3. Typography

| Use | Size | Weight | Transform | Tracking | Color |
|---|---|---|---|---|---|
| Slide title (`h1`) | clamp(28–42px) | 700 | none | -0.02em | `--ink`; `<em>` → `--accent-ink` |
| Section heading (`h2`) | 19px | 700 | none | -0.01em | `--ink` |
| Eyebrow / kicker | 11px | 600 | UPPER | 0.16em | `--accent-ink` |
| Card / panel header | 11px | 600 | UPPER | 0.08em | `--ink-2` |
| Stat label | 10px | — | UPPER | 0.08em | `--ink-3` |
| Stat value | 30px | 700 | none | — | `--ink` (mono); `.o`→`--accent-ink`, `.ok`→`--ok` |
| Field label | 11px | 600 | UPPER | 0.08em | `--ink-3` |
| Lede / body | 13–14px | 400–500 | none | — | `--ink-2` |
| Microlabel / caption | 10–11px | 600 | UPPER | 0.08em | `--ink-3` |

**Number rule:** any element holding numbers gets `.mono` (`font-variant-numeric:tabular-nums`). Headline title accents go in `<em>` recolored to `--accent-ink` (not italic).

**Eyebrow pattern** — small uppercase kicker with a short accent bar:
```html
<div class="eyebrow"><span class="bar"></span>SECTION LABEL</div>
```
```css
.eyebrow{display:flex;align-items:center;gap:8px;font-size:11px;font-weight:600;
  letter-spacing:.16em;text-transform:uppercase;color:var(--accent-ink)}
.eyebrow .bar{width:22px;height:2px;background:var(--accent)}
```

---

## 4. The Deck Shell

The deck fills the viewport between two fixed chrome bars; slides absolute-position between them.

- **`.brandbar`** — fixed top, 38px, `--bg-chrome`, 1px bottom border. Left: 18px THD logo (`THD.png`) + uppercase title (`The Home Depot · Advanced Analytics`, 11px/700/0.1em, `<em>` suffix in `--ink-3`).
- **`.brandstrip`** — fixed 3px under the brand bar; the brand gradient (§2).
- **`.statusbar`** — fixed bottom, 34px, `--bg-chrome`, 1px top border, mono 12px `--ink-3`. A `.ldot` (green, soft ring) signals "running"; `<span>`s separated by 1px `.sep` dividers; a `.right` group (`margin-left:auto`) holds the slide position `NN / NN`.
- **`#deck`** — absolute, inset between chrome (`top:41px; bottom:34px`). Each `.slide` is `position:absolute; inset:0; display:none; flex-direction:column`; `.slide.active{display:flex}`. Inside, `.pad` is the content frame: `flex:1; padding:30px 54px 22px; overflow:hidden`.

Keep the brand bar + status bar + brand strip — that chrome carries the identity. Slides never scroll; size content to fit.

---

## 5. Components

### Card / Panel
```css
.card{background:var(--bg-paper);border:1px solid var(--line);border-radius:var(--r-card);overflow:hidden}
.card-head{min-height:32px;padding:7px 12px;display:flex;align-items:center;gap:8px;
  border-bottom:1px solid var(--line-2);background:var(--bg-stripe);
  font-size:11px;font-weight:600;text-transform:uppercase;letter-spacing:.08em;color:var(--ink-2)}
.card-body{padding:14px}
```
An optional leading **accent dot** (`.dot-r`, 7px, `--accent`) sits in the header. A header `.tail` (`margin-left:auto`, mono, `--ink-3`) holds a right-aligned status/value. Interactive panels reuse this as `.panel` / `.panel-head` with a `.cbody` body.

### Stat tile (the signature KPI display)
```css
.stat{border:1px solid var(--line);background:var(--bg-paper);border-radius:var(--r-card);padding:12px 14px}
.stat .label{font-size:10px;text-transform:uppercase;letter-spacing:.08em;color:var(--ink-3)}
.stat .value{font-variant-numeric:tabular-nums;font-size:30px;font-weight:700;color:var(--ink);margin-top:4px;line-height:1}
.stat .value .u{font-size:15px;color:var(--ink-3);font-weight:600}      /* unit suffix */
.stat .value.o{color:var(--accent-ink)}  .stat .value.ok{color:var(--ok)}
.stat .ctx{font-size:11px;color:var(--ink-3);margin-top:8px;line-height:1.4}
.stat .ctx b{color:var(--ink-2);font-weight:600}
```
Big mono number, tiny uppercase label above, a context line below. Numbers **count up** on slide entry (§7). Don't pair an accuracy number with a progress bar — a value like "96%" is a win, not progress-to-100; the number alone carries it.

### Chip
```css
.chip{display:inline-flex;align-items:center;gap:6px;height:21px;padding:0 9px;border-radius:11px;
  font-size:10px;font-variant-numeric:tabular-nums;font-weight:600;background:var(--bg-hover);
  color:var(--ink-2);border:1px solid var(--line-2);text-transform:uppercase;letter-spacing:.05em}
.chip.ok{background:var(--ok-soft);color:var(--ok);border-color:transparent}
.chip.accent{background:var(--accent-tint);color:var(--accent-ink);border-color:transparent}
```
Status dot (`.cdot`, 7px): `.b` (`--ink-3`, past/built), `.p` (`--ok`, present/in-flight), `.f` (`--accent`, future).

### Buttons
`.btn`: 34px, `--accent` bg, white text, `--r-input` radius, 700/12.5px. Hover → `--accent-ink`; active → `translateY(1px)`. `.btn.ghost`: white bg, `--accent-ink` text, 1px `--line` border; hover `--bg-hover`. One filled-orange button per panel — the headline action.

### Inputs / range
Sliders use `accent-color:var(--accent)`, 4px height. Field label above each control in microlabel style with a right-aligned mono `.val` in `--accent-ink`.

### Phase columns (Build → Evolve → Expand)
Three hairline columns, each `.phasecol` = 1px border + `--bg-stripe` header (`.phasehd`) with an uppercase phase label and a right-aligned `.sub`. The **future** column gets `border-color:var(--accent-soft)` and an `--accent-tint` header — never a left rail or a filled wash. Rows are `.engine` cards (1px `--line-2`, 4px radius) with a leading status dot and an `.m` accent metric span.

---

## 6. Interactive Demo Panels

Slides can host live canvas demos in the two-column `.lab` grid (`312px` control panel + flexible chart panel), both `.panel`s.

- **Control panel:** `.field` sliders, segmented `.seg` buttons (active = filled orange), toggle `.sw` switches (on = `--ok` green), a primary `.btn`, optional `.signal` rows (toggleable, with a delta colored `.up`/`.down`).
- **Chart panel:** a `<canvas>` drawn with hi-DPI `fit()`; readouts below in a `.readout` strip of `.ro` cells (mono 20px value `.up`/`.down`/`.acc`, uppercase `.k` label). Canvas styling matches tokens: `--line` grid, `--accent` series, dashed neutral baselines, soft `rgba(249,99,2,…)` fills.
- **Agent chain:** stepped `.step` rows that light up sequentially (`.on` → `--accent-tint`; a `.gate` step → `--warn-soft`); a live `.feed` strip with a blinking `.pulse`.

Canvas text uses Manrope; numbers stay tabular. Keep the same hairline/mono/uppercase texture inside canvases.

---

## 7. Motion

Restrained and **purposeful** — entrances and state changes only. Never decorative loops (no drifting particles, no idle floats).

- **Slide entrance:** elements tagged `.en` fade-up (`opacity` + 8px `translateY`) over 400ms on `cubic-bezier(.16,1,.3,1)`, staggered via `.d1`–`.d4` delays (~50ms steps). Sibling groups use `.stag` (nth-child delays).
- **Count-up:** stat values animate from 0 to target (~900ms, ease-out) on slide entry, formatted with `toLocaleString` (thousands separators, fixed decimals).
- **Micro-transitions:** buttons `transform 80ms` (press) + `background 120ms`; hovers swap `--bg-hover` in 120ms.
- **Attention pulse:** `@keyframes blink{50%{opacity:.35}}` on live/hint dots, used sparingly.
- **Demo reveals:** agent-chain steps add `.on` on a `setTimeout` cascade; canvases redraw on interaction, not on a RAF loop.
- **Respect reduced motion:** a global `@media (prefers-reduced-motion:reduce)` kills animations/transitions and forces `.en`/`.stag` children visible. Function never depends on animation.

---

## 8. Navigation

A single floating **pill**, centered at the bottom, `box-shadow:var(--shadow-window)` (the only in-page shadow):

```css
#nav{position:fixed;bottom:44px;left:50%;transform:translateX(-50%);
  display:flex;align-items:center;gap:14px;background:#fff;border:1px solid var(--line);
  border-radius:99px;padding:6px 10px;box-shadow:var(--shadow-window)}
```
- Prev/next: 30px circular buttons, hover fills `--accent`.
- **Dot strip:** 8px round dots, `--line-strong`; active dot is **orange and elongated to a 22px pill** (`width` + `background` transition). Click a dot to jump.
- Counter: `NN / NN`, mono 11px `--ink-3`, divided from the dots by a 1px `--line-2`. No "powered by," no vendor mark.

Keys: `→`/`PageDown`/`←`/`PageUp`/`Space` advance (ignored while focus is in an input).

---

## 9. Do / Don't

**Do**
- White paper, 1px hairline borders, uppercase micro-labels, mono tabular numbers.
- One accent — THD orange `#F96302` — for the single active/primary element per region; `--ok` green for "proven / in production."
- Big mono stat value + tiny uppercase label; count it up once on entry.
- Animate every entrance with the `.en`/`.stag` fade-up; respect `prefers-reduced-motion`.
- Soft semantic tints (`*-soft`, `--accent-tint`) for status backgrounds and active rows.

**Don't**
- ❌ Left-edge colored rails / vertical accent strips on cards (hard rule). Use a tinted header, soft border, or a leading dot.
- ❌ Drop shadows on in-page elements (only the nav pill / popovers).
- ❌ Gradients anywhere except the 3px brand strip.
- ❌ Large accent fills / hero washes / glassmorphism.
- ❌ Donut/radial gauges or a progress bar on an achievement number (implies "fell short").
- ❌ Decorative motion (looping particles, idle floats). Motion is entrance + state only.
- ❌ Sans numbers in data contexts — always mono + tabular.
- ❌ Sentence-case or oversized field labels — uppercase, 10–11px, tracked.

---

## 10. Quick-start checklist

1. Paste the `:root` token block (§2). Load Manrope (400–800).
2. Body: white bg, 14px, `tnum`/`ss01`, antialiased, `overflow:hidden`.
3. Shell: `.brandbar` (logo + uppercase title) → 3px `.brandstrip` → `#deck` → `.statusbar` (live dot + slide position).
4. Slides: `.slide` absolute, `.active` shows; `.pad` content frame; size to fit, never scroll.
5. Eyebrow → `h1`/`h2` → lede → content, each tagged `.en .dN` for staggered entrance.
6. Stats: `.stat` tiles (mono value + uppercase label + context line); `data-count` for count-up.
7. Cards/panels: `.card`/`.card-head`/`.card-body`; uppercase header on `--bg-stripe`, optional `.dot-r`.
8. Phase columns: 3 `.phasecol`s; future column gets `--accent-soft` border + `--accent-tint` header (no rail).
9. Demos: `.lab` two-column, `.panel`s, canvas drawn with token colors + tabular readouts.
10. Nav: one centered `#nav` pill — arrows + elongated-orange-dot strip + `NN / NN` counter. No vendor mark.
```
