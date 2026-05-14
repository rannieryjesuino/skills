# Verdant Obsidian — Graphite Variant

A dark, graphite-first design system with restrained green accents.

## Intent

The page should feel like a premium internal technical document:

- dark graphite base;
- high readability;
- green as identity/accent;
- clear hierarchy;
- subtle borders;
- no excessive neon;
- no green-dominant backgrounds.

## Core tokens

```css
:root {
  --bg: #0B0D0E;
  --bg-elevated: #111417;
  --surface: #171B1E;
  --surface-2: #1D2326;
  --surface-3: #22292D;

  --border: #2A3235;
  --border-accent: #1F6F4A;

  --text: #E8ECEA;
  --text-muted: #A9B3AF;
  --text-soft: #7F8B86;

  --accent: #36E89F;
  --accent-2: #2DAE78;
  --accent-soft: rgba(54, 232, 159, 0.12);
  --accent-border: rgba(54, 232, 159, 0.35);

  --info: #6AD8FF;
  --info-soft: rgba(106, 216, 255, 0.12);

  --warning: #E8BD66;
  --warning-soft: rgba(232, 189, 102, 0.12);

  --danger: #FF6B86;
  --danger-soft: rgba(255, 107, 134, 0.12);

  --shadow: 0 18px 50px rgba(0, 0, 0, 0.35);
  --radius-lg: 18px;
  --radius-md: 12px;
  --radius-sm: 8px;
}
```

## Usage rules

### Backgrounds

Use these for backgrounds:

- page background: `--bg`;
- sidebar/topbar: `--bg-elevated`;
- cards: `--surface`;
- nested cards/diagrams: `--surface-2`;
- active states: use `--accent-soft`, not a full green fill.

Avoid using green as a large background.

### Text

Use:

- `--text` for main text;
- `--text-muted` for descriptions;
- `--text-soft` for labels, metadata, timestamps.

### Accent

Use green only for:

- active navigation state;
- section markers;
- links;
- arrows;
- success states;
- selected badges;
- primary buttons;
- small highlights.

### Status colors

- Info: `--info`
- Warning: `--warning`
- Danger: `--danger`
- Success/primary: `--accent`

Always pair status colors with text labels.

## Layout patterns

Preferred page layout:

- left sidebar: 240px to 280px;
- main content: fluid grid;
- topbar: sticky;
- cards: 16px to 24px padding;
- grid gap: 16px;
- max text width: around 72ch for prose sections.

## Typography

Recommended stack:

```css
font-family:
  Inter,
  ui-sans-serif,
  system-ui,
  -apple-system,
  BlinkMacSystemFont,
  "Segoe UI",
  sans-serif;
```

Use monospace for technical identifiers:

```css
font-family:
  "SFMono-Regular",
  Consolas,
  "Liberation Mono",
  Menlo,
  monospace;
```

## Visual density

Make pages information-rich but not crowded.

Prefer:

- short paragraphs;
- compact cards;
- strong headings;
- tables for comparisons;
- diagrams for flows;
- checklists for next steps;
- warning cards for risks.

Avoid:

- decorative-only gradients;
- excessive glow;
- large green panels;
- tiny text;
- walls of prose.
