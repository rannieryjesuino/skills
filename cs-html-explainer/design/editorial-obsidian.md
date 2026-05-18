# Editorial Obsidian

A dark, editorial design system with disciplined lime accent and deep-teal surfaces. Built for technical artifacts that read like a premium magazine spread instead of a developer dashboard.

## Intent

The page should feel like a long-form editorial piece:

- warm near-black background, never pure `#000`;
- deep teal surfaces for elevation, not lighter graphites;
- huge serif headlines with one keyword italicized in lime;
- generous whitespace (60–70% of the page is empty);
- lime green used with extreme discipline (max 5 occurrences per page);
- left-aligned everything (centering reserved for incident-report hero only);
- hairlines and typographic kickers instead of framed boxes everywhere.

## Core tokens

```css
:root {
  /* Surfaces — dark + warm */
  --bg:               #1A1615;   /* page background — warm near-black, NEVER #000 */
  --surface:          #243833;   /* primary card — deep teal */
  --surface-inverted: #FCF8EC;   /* off-white paper — once per page maximum */
  --surface-forest:   #0F352B;   /* deep institutional — hero band / root-cause only */
  --pure-black:       #000000;   /* borders, icon strokes, danger bar */

  /* Accents */
  --accent-lime:      #DBFFA5;   /* CTA + ONE italicized H1 keyword */
  --accent-lime-hover:#C5F088;   /* darkened lime for CTA hover */
  --accent-moss:      #274E13;   /* arrows, secondary borders, code bar */

  /* Text */
  --text:             #FFFFFF;
  --text-muted:       #9E9E9E;
  --text-meta:        #666666;
  --text-inverted:    #1A1615;

  /* Lines */
  --hairline-alpha:   rgba(239, 239, 239, 0.10);
  --border-card:      rgba(255, 255, 255, 0.10);

  /* Typography stacks */
  --font-serif: "Playfair Display", Georgia, "Times New Roman", serif;
  --font-sans:  "Geist", ui-sans-serif, system-ui, -apple-system, "Segoe UI", sans-serif;
  --font-mono:  "Geist Mono", ui-monospace, SFMono-Regular, Consolas, Menlo, monospace;

  /* Spacing (respiro scale) */
  --space-xs: 8px;
  --space-sm: 16px;
  --space-md: 32px;
  --space-lg: 64px;
  --space-xl: 96px;
  --space-2xl: 120px;

  /* Radius — flat editorial */
  --radius-card: 4px;
  --radius-pill: 999px;

  /* Sizes */
  --max-content: 1200px;
  --sidebar-width: 220px;
  --topbar-height: 56px;
}
```

## Usage rules

### Backgrounds

- Page background: `--bg`.
- Cards / elevated zones: `--surface`.
- Inversion (paper): `--surface-inverted` — ONCE per page maximum (TL;DR card, single key-insight callout, or rollback block).
- Forest green `--surface-forest`: only for the incident-report root-cause card or an optional hero band texture.
- Lime is NEVER a background larger than a CTA pill.

### Lime discipline

`--accent-lime` may appear in at most **5 places** on a single page:

1. ONE italicized keyword inside the H1 (the "marca-texto").
2. The primary CTA pill.
3. The active sidebar item (2px left border only — no fill).
4. Inline links (underline on hover).
5. The `SUCESSO` status label (text + bullet glyph).

Forbidden:

- Lime as a card background.
- Lime borders on multiple cards in the same section.
- Lime inside tables (header, row, or cell).
- Two italicized lime keywords in the same heading.

### Status signaling

No traditional UI reds or yellows. Map status to warm-palette + uppercase typographic kickers. Color must always be paired with a typographic uppercase label.

| Status | Treatment | Label |
|---|---|---|
| Success | `●` glyph in `--accent-lime` + uppercase Geist SemiBold 11px in lime | `SUCESSO` / `OK` |
| Warning | `▲` glyph in `--surface-inverted` (#FCF8EC) + uppercase Geist SemiBold 11px in off-white | `ATENÇÃO` |
| Danger  | 4×16px vertical bar in `--pure-black` (left of label) + uppercase Geist SemiBold 11px in `--text` (white) | `RISCO ALTO` |
| Neutral / Info | uppercase Geist SemiBold 11px in `--text-meta` (no glyph) | `NOTA` |

### Borders

Prefer hairlines (1px at low opacity) to framed boxes. A card carries at most one 1px border in `--border-card`. Inter-section dividers are full-bleed 1px lines in `--hairline-alpha`, NOT card edges.

### Text colors

- `--text` (white) for body copy on dark surfaces.
- `--text-muted` (#9E9E9E) for subtitles and secondary body.
- `--text-meta` (#666666) for captions, timestamps, and footnotes only — NEVER body text (fails AA on `--bg`).
- `--text-inverted` for any text on `--surface-inverted` paper blocks.

## Typography rules

| Level | Family | Size | Weight | Notes |
|---|---|---|---|---|
| H1 | Playfair Display | `clamp(48px, 6vw, 80px)` | 400 | letter-spacing `-0.02em`, line-height `1.05`. ONE `<em>` in italic colored `--accent-lime`. |
| H2 | Playfair Display | `clamp(32px, 3.2vw, 44px)` | 500 | letter-spacing `-0.015em`, line-height `1.1`. |
| H3 | Playfair Display | 24px | 500 | letter-spacing `-0.01em`. |
| Subtitle / dek | Geist | 20–22px | 300 | color `--text-muted`, max 60ch, sits 32–40px below H1. |
| Body | Geist | 16px | 400 | line-height `1.6`, max 68ch. |
| Kicker / eyebrow | Geist | 10–11px | 600 | UPPERCASE, letter-spacing `0.08em`, color `--accent-lime` (hero) or `--text-meta` (elsewhere). Max 2 words. Sits 16px above the title it labels. |
| Caption / metadata | Geist | 12px | 400 | color `--text-meta`. |
| Code / mono | Geist Mono | 14px | 400 | 4px left bar in `--accent-moss`, no border, same surface as parent card. |

### The italicized lime keyword

Every H1 must contain exactly ONE `<em>` element. The italicized word is colored `--accent-lime` and is the only chromatic element in the hero. This is the editorial signature of the system — never skip it, never double it.

Example: `Decidimos <em>migrar</em> o pipeline de eventos.`

## Font loading (hybrid)

```html
<link rel="preconnect" href="https://fonts.googleapis.com" crossorigin>
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,500;1,400&family=Geist:wght@300;400;600&family=Geist+Mono:wght@400&display=swap" rel="stylesheet">
```

The variable stacks `--font-serif`, `--font-sans`, `--font-mono` already declare fallbacks (Georgia, system-ui, SFMono). If the CDN is blocked, fonts degrade gracefully — the page must remain legible and on-brand offline.

## Layout patterns

- Sidebar: 220px sticky, same background as page, 1px right border in `--border-card`.
- Main content: max-width `--max-content` (1200px), generous side padding.
- Topbar: 56px, transparent, hairline bottom, CTA right-aligned.
- Card padding: 32–40px (editorial breathing).
- Grid gap between cards: 24px minimum.
- Vertical rhythm (the respiro scale): 16px → 32px → 64px → 96px → 120px between major blocks.

## Visual density

Editorial respiro. Each section should feel like turning a magazine page, not scanning a dashboard.

Prefer:

- single-column body, left-aligned;
- big serif headlines that breathe;
- hairlines instead of card frames;
- 1–2 structural cards per long section, never a wall of cards;
- typographic emphasis (italic, uppercase kicker) instead of color;
- generous gaps between hero, sections, and footer.

Avoid:

- crowded heroes;
- gradients;
- excessive shadows;
- centered body copy;
- lime as decoration;
- red/yellow status pills;
- Inter or any sans that isn't Geist;
- pure-black `#000` page backgrounds.
