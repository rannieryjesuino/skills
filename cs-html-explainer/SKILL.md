---
name: cs-html-explainer
description: Use when the user wants an editorial dark-mode HTML artifact (huge serif headlines, generous whitespace, deep-teal surfaces, lime accent) for system explainers, ADR/DRE pages, implementation plans, incident reports, module maps, or executive-style handoff docs. Generates a single self-contained HTML file with the "Editorial Obsidian" identity — magazine-style respiro, Playfair Display + Geist typography, hairline dividers, native HTML/CSS interactivity (disclosure, CSS tabs/steppers, hover highlight), and a strict 5-occurrence lime budget per page.
version: 2.1.0
license: MIT
metadata:
  tags: [html, documentation, explainer, adr, dre, postmortem, module-map, editorial, magazine]
---

# Editorial HTML Explainer Skill

## Overview

Use this skill to turn technical context into a single self-contained HTML page styled like an editorial magazine spread — huge Playfair Display headlines, generous whitespace, deep-teal surfaces, hairline dividers, and a single disciplined lime-green accent.

The output should feel like turning a magazine page, not scanning a developer dashboard.

Prefer Markdown for source documentation, KB ingestion, PR diffs, and short notes. Use this skill when the user wants a polished, navigable artifact that reads like a long-form editorial piece.

## Core output contract

When this skill is invoked, generate exactly one complete `.html` file unless the user explicitly asks for multiple files.

The HTML must be:

- self-contained for offline reading (body, layout, and components render correctly with no network);
- dark-mode (warm near-black `#1A1615`, never pure `#000`);
- responsive (collapses to a single column at 980px);
- accessible (WCAG AA contrast);
- structured around the marginalia / spread / band patterns described below — NEVER a sidebar+topbar SaaS chrome;
- written for humans who need to grasp a system or decision quickly.

### Font loading

Google Fonts CDN is allowed — and recommended — for loading Playfair Display, Geist, and Geist Mono. The page must remain legible and on-brand offline via the fallback stack (Georgia, system-ui, SFMono). Use the `<link>` block documented in `design/editorial-obsidian.md`. Do not load any other external resource (images, CSS, JS, icons).

## Default visual style — Editorial Obsidian

See `design/editorial-obsidian.md` for the full token set and usage rules.

Quick summary:

- Page background: `--bg` `#1A1615` (warm near-black, never `#000`).
- Code / dense panel surface: `--surface` `#243833` (deep teal — used only inside `<pre class="code">` and similar dense blocks, NOT as a card background everywhere).
- Accent: `--accent-lime` `#DBFFA5`, used in at most 5 places per page.
- Inversion to paper: `--surface-inverted` `#FCF8EC` — used in the TL;DR band and (in implementation-plan) the rollback band.
- Forest green: `--surface-forest` `#0F352B` — incident-report root cause only.
- Pure black `#000000`: only for the danger status bar.
- Typography: Playfair Display for serif headlines, Geist for body/UI, Geist Mono for code and timestamps.

The page should feel like an editorial spread, not a gaming UI and not a SaaS dashboard.

## When to use

Use when the user asks for any of the following (in any language):

- an editorial / magazine / deck-style HTML page;
- a "more visual" or "more polished" version of a technical document;
- a serif-headline, Playfair, or "respiro" visual treatment;
- a visual explanation of a system, flow, or integration;
- a DRE (human-friendly version of an ADR);
- a visual implementation/migration/rollout plan;
- a visual postmortem or incident report;
- a module map, onboarding page, or codebase walkthrough;
- an executive-style handoff document.

Do not use this skill when the user only wants a short answer, a Markdown doc, a code snippet, a README, or text for a ticket.

## Supported document modes

Five modes. Each mode shares the same scaffold (masthead + hero + lede + TL;DR + sections + footer) but uses a unique signature layout.

### 1. `system-explainer`

How a system, flow, integration, service, or feature works.

Recommended sections: TL;DR · Problema · Fluxo · Componentes · Dados · Operação · Falhas · Troubleshooting · FAQ · Próximos passos.

Template: `templates/system-explainer.html`.

Mode-specific element: a horizontal `.flow` diagram between the TL;DR and the rest of the page — 5-column grid, hairlines as gutters, italic-serif node names. This is the visual signature of this mode.

### 2. `architecture-decision`

ADRs, DREs, trade-off analysis, decision explanation. Generates a visual DRE; if the user asks for a source ADR too, include a compact markdown ADR block inside `<pre class="code">`.

Recommended sections: TL;DR · Contexto · Forças em conflito · Decisão · Por que venceu · Opções consideradas · Trade-offs · Consequências (+/−) · Riscos · Impacto · Antes/Depois · ADR markdown · Próximos passos.

Template: `templates/architecture-decision.html`.

Mode-specific elements:
- The page is rendered as a TWO-PAGE EDITORIAL SPREAD: left column narrative (context, decision, why it won, Antes/Depois), right column apparatus (options, trade-offs, consequences, risks, ADR source).
- Options consideradas is a VERTICAL stacked list (`.options`), not a 4-column table. The chosen option is rendered in italic lime.
- Antes/Depois is a vertical stack with a typographic arrow band (`DECISÃO TOMADA`) between the two panels.

Status values: `PROPOSTO`, `ACEITO`, `SUPERADO`, `DEPRECIADO`, `REJEITADO` — always uppercase Geist, never pill-shaped.

### 3. `implementation-plan`

Migrations, refactors, rollout plans, technical delivery strategy.

Recommended sections: TL;DR · Waves band · Objetivo · Estado atual/alvo · Escopo · Não-objetivos · Plano passo-a-passo · Riscos · Rollback · Validação · Donos.

Template: `templates/implementation-plan.html`.

Mode-specific elements:
- A horizontal WAVES BAND right after the TL;DR: a typographic ruler with week ticks (S1…S7) and three wave cards. The critical wave gets the lime accent (`.wave.critical`); the others stay in `--text` white.
- The rollback section is a full-bleed `.paper-band` — inverted paper, oversized italic-lime keyword in the H2, a `.stamp` footer with the rollback SLA.

### 4. `incident-report`

Postmortems, debugging narratives, failure analysis.

Recommended sections: TL;DR · Impacto · Resumo · Timeline · Detecção · Causa raiz · Fatores · O que funcionou · O que falhou · Ações corretivas · Ações preventivas · Questões abertas.

Template: `templates/incident-report.html`.

Mode-specific elements:
- This is the ONLY mode allowed to use the centered `.hero.front` variant. Hero kicker carries a severity badge (danger black bar) inline.
- An `Impact band` with three oversized Playfair `.stat` numbers separated by vertical hairlines — only the most severe metric gets `.stat.lime`.
- A `.timeline-list` with a vertical rail and three marker variants: default (gray), `.critical` (black fill — for the failure inflection points), `.lime` (lime fill — for mitigation / resolution).
- The root cause is a full-bleed `.forest-band`. This is the only place where `--surface-forest` appears in the entire output.

### 5. `module-map`

Codebase/module explanation, service ownership maps, dependency maps, onboarding pages.

Recommended sections: TL;DR · Entry points · Visão do módulo · Diretórios · Abstrações · Dependências · Fluxo de dados · Extensão · Mudanças comuns · Riscos · Testes.

Template: `templates/module-map.html`.

Mode-specific elements:
- An `Entry points` band right after the TL;DR — oversized Playfair links with italic-lime HTTP verbs / worker prefixes.
- An asymmetric body grid: a STICKY file tree on the left (with `--accent-moss` left bar) that scrolls with the content column on the right.

## Mandatory page structure

Every generated HTML artifact must include:

1. **Masthead** (sticky, three columns)
   - Left: volume / number / mode label (`Vol. 01 · Nº 03 — Plano de execução`).
   - Center: wordmark `Editorial Obsidian` (italic Playfair).
   - Right: ghost `Exportar / PDF` button + lime `Copiar resumo` CTA pill.

2. **Hero**
   - Default: 2-column grid with a `.left-rail` of metadata + huge H1 Playfair with ONE `<em>` in lime.
   - Alternative (single-column hero with `.eyebrow-row` + `.hero-meta` 4-column ledger): used by ADR, implementation-plan, and module-map.
   - Centered variant (`.hero.front`): incident-report only.

3. **Lede row** (optional but recommended for the default hero)
   - 2-column grid: marker label "Sinopse" + a single 20–28px paragraph in `--text-muted`.

4. **TL;DR paper band**
   - One `.tldr-paper` (off-white inversion). This is the single inversion of the page in most modes.

5. **Mode-specific signature block**
   - Each mode has one — do not skip it.

6. **Sections** with marginalia
   - Inside `<main class="sections">`. Each `.section` is a 2-column grid: rail (number + topic + small `.ref` line) on the left, content on the right.
   - First section gets a `--hairline-strong` top border; subsequent sections use `--hairline-alpha`.

7. **Optional full-bleed bands**
   - `.paper-band` (implementation-plan rollback).
   - `.forest-band` (incident-report root cause).

8. **Footer footnote**
   - Mono 11px, two columns: `Editorial Obsidian — cs-html-explainer` on the left, `Folio NN / 05 · {{MODE_KICKER}}` on the right.
   - Separated from the body by a `--hairline-strong` top border.

What is FORBIDDEN in the new structure:

- A 220px sticky sidebar with a nav `<aside>`. The marginalia rail + the masthead replace it.
- A 56px sticky topbar with right-aligned ghost+CTA buttons. The masthead replaces it.
- `<hr class="divider">` between every section. Section borders are part of `.section` itself.
- Framed cards (`.card`) as the default container. Prefer hairlines and full-bleed bands.

## Editorial respiro rule

Apply the "60–70% of the page is empty" rule literally:

- Masthead to hero: clamp(72px, 12vw, 180px) padding-top.
- Hero to lede row: lede sits inside its own row with `--space-lg` padding-top after a `--hairline-strong` divider.
- Lede to TL;DR: clamp(48px, 6vw, 96px) padding-bottom on the lede-row.
- TL;DR to next band: clamp(48px, 6vw, 96px) padding-bottom on the tldr-band.
- Between sections: clamp(48px, 6vw, 96px) padding top AND bottom.
- Full-bleed bands: clamp(64px, 10vw, 140px) vertical padding, clamp(48px, 6vw, 96px) external margin.
- Outer container: max-width 1440px, gutter clamp(24px, 5vw, 80px).
- Body alignment: ALWAYS left. Centering is allowed only for the incident-report `.hero.front`.

Use the spacing scale tokens (`--space-sm` through `--space-2xl`) — do not invent ad-hoc values for between-section spacing.

## Interactivity rules

Editorial Obsidian is a reading artifact, but a reading artifact can still be didactic. Whenever a section would benefit from progressive disclosure, lateral navigation, or visual emphasis on relationships, prefer an interactive treatment over a wall of static text.

**Default posture: actively look for places to add native interactivity.** A page with zero interactive affordances is a missed opportunity if any of the following exists: a long FAQ, more than three "options considered", a timeline with many entries, a flow diagram with named nodes, a troubleshooting list, or a stepper-shaped plan. The rule is "incentivar" — when in doubt, add it.

### Hard constraint: zero JavaScript

All interactivity must be implemented with native HTML and CSS only. No `<script>` blocks. No inline event handlers. No libraries. If a behavior cannot be expressed with semantic HTML + CSS (`:target`, `:checked`, `:hover`, `:focus-visible`, `:has()`, sibling/adjacent combinators), it does not belong in this skill.

This preserves the four invariants of the artifact: self-contained for offline reading, printable, accessible by default, and reviewable without trusting executable code.

### Approved interactive patterns

Three patterns are blessed. Use them deliberately — not every section needs interactivity, but every page should have at least one of these unless the content is genuinely too short to justify it.

#### 1. Disclosure (`<details>` / `<summary>`)

Native progressive disclosure. Default for any list where the headline is enough for skimmers but readers may want depth.

When to use:
- FAQ items (one `<details>` per question).
- Troubleshooting steps (symptom in `<summary>`, diagnosis + fix in the body).
- "Options considered" in ADR/DRE — collapse the rejected ones, leave the chosen option open by default (`<details open>`).
- Timeline entries with optional deep-dive context.
- "Falhas conhecidas" / "Premissas" / "Questões abertas" lists.

Constraints:
- The `<summary>` must be self-contained: a reader who never expands it should still understand the gist.
- Style `<summary>` as a marginalia row (left rail kicker + Playfair H3-equivalent on the right). Do NOT use the default browser triangle — replace with a typographic glyph (`+` / `−`) that flips via `[open] > summary::before`.
- Keep one disclosure per row, hairline divider between rows, no framed cards.
- Never put the TL;DR, the H1, or the mode-specific signature block inside a `<details>`. Skimmers must see them without interaction.

#### 2. CSS tabs and steppers (`:target` or `:checked`)

Lateral navigation between equivalent views. Default for content that is "one of N alternatives at a time" rather than "all of N in sequence".

When to use:
- Implementation-plan waves: tabs for Wave 1 / Wave 2 / Wave 3, only one panel visible.
- ADR options considered: tab strip with one option per tab, chosen option pre-selected.
- System-explainer "Fluxo" alternatives (happy path vs. failure path vs. retry path).
- Module-map "Mudanças comuns" cookbook entries.

Implementation guidance:
- Prefer the `:target` pattern when the tabs map to permalinkable views (a stakeholder can share a URL pointing at the active tab).
- Use the radio + `<label>` + `:checked` pattern when permalinkability does not matter and you need a default-selected tab on first load.
- The tab strip itself is typographic: uppercase Geist kickers separated by hairlines, the active tab gets a 2px lime bottom border (this counts toward the lime budget — see the per-mode allocations).
- Provide visible `:focus-visible` outlines on tab triggers (`<a>` for `:target`, `<label>` for radio).
- Always render a print stylesheet that expands every tab into a stacked, fully visible view (`@media print { .tab-panel { display: block !important } }`). The artifact must remain complete on paper.

#### 3. Hover and focus highlight on diagrams

Relational emphasis. Default for any `.flow` diagram or dependency map where nodes share connections that are not obvious at rest.

When to use:
- `.flow` between TL;DR and the rest of a system-explainer page.
- Module-map dependency graphs.
- ADR "Antes/Depois" panels (hovering a "before" element highlights the corresponding "after" element).
- Incident-report timeline (hover a marker, surface the related action item below).

Implementation guidance:
- Use sibling/adjacent combinators (`.flow:hover .node:not(:hover)`) or `:has()` to dim siblings of the hovered node.
- Pair every `:hover` with `:focus-within` (or `:focus-visible` on a focusable child) so keyboard users get the same affordance.
- Highlight must not be color-only — combine with weight, italic, or a hairline thickness change so the cue survives in monochrome.
- Never use lime as the highlight color (it would blow the budget). Use `--text` white going to `--text-muted` gray on non-hovered siblings, or thicken the hairline on the active node.
- The diagram must remain fully legible at rest. Hover is enrichment, not a prerequisite for understanding.

### Mode-specific guidance

| Mode | Strongly recommended | Optional | Skip |
|---|---|---|---|
| `system-explainer` | Disclosure for FAQ, Troubleshooting, Falhas | Hover/focus highlight on `.flow` | Tabs (the page is a single narrative) |
| `architecture-decision` | Disclosure for rejected options; tabs for "Opções consideradas" if there are 3+ | Hover/focus on Antes/Depois | Disclosure on the chosen option (must stay open) |
| `implementation-plan` | Tabs/stepper for waves; disclosure for per-wave detail | Disclosure for "Riscos" with mitigation depth | Hiding the rollback `.paper-band` behind a disclosure |
| `incident-report` | Disclosure for "O que funcionou / O que falhou / Questões abertas" | Hover/focus on timeline ↔ actions | Disclosure on the impact band, timeline markers, or root-cause `.forest-band` |
| `module-map` | Disclosure for "Mudanças comuns" cookbook; tabs across cookbook recipes | Hover/focus on dependency diagram | Disclosure on Entry points or the sticky file tree |

### Forbidden interactivity

Even with the patterns above, do NOT:

- Hide the TL;DR, hero, masthead, mode-specific signature block, or footer footnote behind any interactive control.
- Use animation longer than 150ms on any interactive transition. No bouncy easings, no glow pulses.
- Apply hover/focus styling that depends on color alone (always pair with weight, italic, or hairline thickness).
- Build a custom accordion in `<div>`s when `<details>` works.
- Introduce a tab pattern where the user actually needs to read all panels in sequence — that is a stepper of full sections, not tabs.
- Build search boxes, sortable tables, or live filters (those require JS or compromise the print/offline contract).

### Accessibility contract for interactive elements

- Every `<details>` / `<summary>` is keyboard-operable by default; do not break that.
- Every CSS-tab trigger must be a real `<a href="#...">` (for `:target`) or `<label for="...">` paired with a visually hidden `<input type="radio">` (for `:checked`). Never fake interactivity with `<div onclick>`-style markup (no `onclick` is allowed anyway — see the hard constraint).
- Provide `:focus-visible { outline: 2px solid var(--accent-lime); outline-offset: 4px; }` on every interactive element. The lime focus ring does NOT count against the 5-occurrence lime budget — it is a transient accessibility affordance, not a static page element.
- Touch targets ≥ 32×32px (summary rows, tab labels).
- Print stylesheet (`@media print`) must expand every collapsed `<details>`, reveal every tab panel, and remove hover-only styling so the artifact prints as a complete document.

## Lime discipline rule

`--accent-lime` `#DBFFA5` may appear in at most **5 places** on a single page. Count them.

Common allocations by mode:

| Mode | Allocations |
|---|---|
| system-explainer | 1) H1 `<em>` · 2) CTA pill · 3) hero left-rail label · 4) `.status.success` label · 5) inline link |
| architecture-decision | 1) H1 `<em>` · 2) CTA pill · 3) `.eyebrow-row` kicker · 4) `.option.chosen` italic title · 5) `.panel.after` tag |
| implementation-plan | 1) H1 `<em>` · 2) CTA pill · 3) `.eyebrow-row` kicker · 4) ONE `.wave.critical` num+tag · 5) `.paper-band` h2 `<em>` |
| incident-report | 1) H1 `<em>` · 2) CTA pill · 3) hero kicker · 4) ONE `.stat.lime` num · 5) 2× lime timeline markers (count as one budget slot — they describe the same arc: mitigation + resolution) |
| module-map | 1) H1 `<em>` · 2) CTA pill · 3) `.eyebrow-row` kicker · 4) up to 2× `.entry-list em` (the third entry uses `.muted-link`) · 5) `.dir-grid h3` (mono lime labels) |

Forbidden everywhere:

- Lime as a card background.
- Lime borders on multiple cards in the same section.
- Lime inside tables (header, row, or cell).
- Two italicized lime keywords in the same heading.
- Lime gradients, glows, or shadows.

If you exceed 5 lime occurrences while generating, demote the lowest-priority ones to `--text` white, `--accent-moss`, or `--text-muted`.

## Status signaling rule

No traditional UI red/yellow. Status is paired color + uppercase typographic label. Mapping:

| Status | Color treatment | Label |
|---|---|---|
| Success / OK | `●` glyph in `--accent-lime`, label in lime | `SUCESSO`, `OK`, `PLANEJADO`, `CONCLUÍDO` |
| Warning / Attention | `▲` glyph in `--surface-inverted` (#FCF8EC), label in off-white | `ATENÇÃO`, `EM ANDAMENTO` |
| Danger / High risk | Vertical 4×14px bar in `--pure-black`, label in white | `RISCO ALTO`, `BLOQUEADO`, `FALHA` |
| Neutral / Info | Uppercase label in `--text-meta`, no glyph | `NOTA`, `PLANEJADO` (neutral) |

Color must always be paired with the typographic uppercase kicker. Never signal status with color alone.

## No source-brand reference rule

The skill output must NOT mention any source brand, company name, internal codename, or product that inspired this visual identity. The masthead wordmark is fixed to `Editorial Obsidian`. The footer footnote left side reads `Editorial Obsidian — cs-html-explainer`.

If the user provides company or product names in the content they want documented, those names are fine inside body text — but they must never appear in skill chrome (masthead, footer, kickers, template comments visible to the reader).

## Accessibility and readability

- All body text must meet WCAG AA contrast on the relevant surface.
  - White `#FFFFFF` on `--bg` `#1A1615` → ≈ 17 (AAA pass).
  - Muted `#9E9E9E` on `--bg` → ≈ 5.2 (AA pass).
  - Meta `#666666` on `--bg` → ≈ 3.0 — acceptable ONLY for non-essential metadata; NEVER for body text.
  - Lime `#DBFFA5` on `--text-inverted` `#1A1615` (CTA / paper band) → ≈ 14 (AAA pass).
- Use semantic HTML: `header`, `main`, `section`, `article`, `aside`, `nav`, `table`, `button`, `details`/`summary`, `time`.
- Provide visible focus states (`:focus-visible { outline: 2px solid var(--accent-lime); outline-offset: 4px; }`).
- Pair color with text or icons — never color alone.
- Avoid animation unless it improves comprehension. Interactive transitions (disclosure expand, tab change, hover highlight) are allowed but must stay ≤ 150ms and use a neutral easing — no bouncy easings, no glow pulses.

## Diagram rules

- Use pure HTML/CSS/SVG. No Mermaid, no D3, no React, no external libraries (unless the user explicitly requests one).
- Boxes and arrows. Arrows in `--accent-moss`, never lime.
- Short labels (max ~30 characters per node).
- Include a short prose paragraph below every diagram explaining what the diagram does NOT show (side effects, async fan-out, retries — whatever is omitted).
- Avoid overcrowding — break into multiple diagrams if you have more than ~6 nodes.
- Prefer adding a hover/focus highlight to relational diagrams (`.flow`, dependency maps, Antes/Depois) — see the Interactivity rules section. The diagram must still be legible at rest; hover is enrichment, not a prerequisite.

## Content rules

Be honest about uncertainty. If information is missing:

- mark it as `Desconhecido`, `Premissa`, or `A confirmar`;
- do not invent owners, dates, environments, metrics, ARNs, URLs, resource names, team names, or API behavior;
- preserve ambiguity instead of filling gaps with fake confidence.

When using user-provided context, do not add external facts unless asked.

## ADR/DRE-specific rules

For architecture decisions:

- document only architecturally significant decisions;
- include alternatives that were considered;
- show why the chosen option won;
- include trade-offs and consequences (positive AND negative);
- keep the optional ADR source block concise (≤ 60 lines);
- use the HTML DRE as the human-friendly presentation layer.

If a decision is superseded, include a link or placeholder to the newer decision when known.

## Output quality checklist

Before finishing, verify:

- The HTML opens as a standalone file (`file://`).
- Page background is `#1A1615` (warm), never pure `#000`.
- The hero H1 has exactly ONE `<em>` element in italic lime.
- Lime occurrences on the page ≤ 5 (count them).
- No red, no yellow, no orange anywhere in the chrome.
- No source-brand name appears in chrome (masthead, footer, kickers).
- TL;DR exists near the top.
- The mode-specific signature block is present (flow / spread / waves band / front-page+stats+forest / entry points + sticky tree).
- The masthead and footer footnote are present (no sidebar+topbar SaaS chrome).
- At least one native HTML/CSS interactive affordance is present where it improves comprehension (disclosure, CSS tabs/steppers, or hover/focus highlight) — see the Interactivity rules section. Skip only if the content is genuinely too short to justify it.
- No `<script>` blocks and no inline event handlers anywhere in the output.
- A `@media print` block expands every `<details>` and reveals every tab panel so the artifact prints as a complete document.
- Unknowns are clearly marked, not invented.
- The artifact is skimmable in under 60 seconds.

## Common pitfalls

1. **Falling back to the old sidebar+topbar chrome.** The new structure is masthead (top) + marginalia sections + footer footnote. No 220px sticky sidebar. No 56px sticky topbar.
2. **Using lime as a background.** Lime is for one H1 keyword, the CTA pill, the active kicker, the chosen-option title, and one accent block per page. Nothing else.
3. **Centering body text.** Always left-aligned. The only exception is the `incident-report` `.hero.front`.
4. **Forgetting to italicize one keyword in H1.** Every page must have exactly one `<em>` in the H1. Not zero, not two.
5. **Using red or yellow UI status colors.** No `#FF6B86`, no `#E8BD66`. Use the warm-palette + uppercase label mapping.
6. **Mentioning the source brand.** The skill is generic. The chrome is fixed to `Editorial Obsidian`.
7. **Crowding the hero.** Apply the respiro scale strictly. Generous padding above the hero, generous padding before the TL;DR.
8. **Two inverted bands at the same weight.** The off-white inversion is an editorial pull-quote. Use `.tldr-paper` for the summary. If you also use `.paper-band` (implementation-plan rollback), make sure they carry different semantic weight (summary vs. exit ramp) so the reader does not perceive them as duplicates.
9. **Lime gradients, glows, or shadows.** Editorial Obsidian is flat. Radius is 4px on cards, 999px only on the CTA pill.
10. **Framed cards (.card with full borders) everywhere.** Default to hairlines and full-bleed bands. Framed cards are allowed only for `<pre class="code">` and similar dense panels.

## Verification checklist

- [ ] Output is exactly one standalone `.html` file unless the user asked for more.
- [ ] No external CDN/CSS/JS dependency was added beyond the Google Fonts `<link>`.
- [ ] Fonts degrade gracefully offline (Georgia / system-ui / SFMono fallbacks).
- [ ] Masthead is present, sticky, three columns.
- [ ] Hero uses the mode-appropriate variant (left-rail / single-column with eyebrow-row / centered `.hero.front` for incident-report).
- [ ] Lede row sits below the hero (when using the default left-rail hero).
- [ ] TL;DR appears near the top as a `.tldr-paper`.
- [ ] The mode-specific signature is present (.flow / two-page spread / waves band / impact stats + timeline-list + forest-band / entry-list + sticky tree).
- [ ] Sections use the marginalia pattern (rail + content), separated by hairlines.
- [ ] H1 contains exactly one italicized lime keyword.
- [ ] Lime usage ≤ 5 occurrences (manually counted).
- [ ] No red, yellow, or orange anywhere in the chrome.
- [ ] Status badges always pair color with uppercase Geist label.
- [ ] No source-brand reference in chrome (masthead wordmark + footer footnote are fixed to `Editorial Obsidian`).
- [ ] Page background is `#1A1615`, never `#000`.
- [ ] Body is left-aligned everywhere except (optionally) the incident-report `.hero.front`.
- [ ] Hairlines separate sections, not framed boxes.
- [ ] At least one native HTML/CSS interactive affordance is present where it adds didactic value (disclosure / CSS tabs / hover-focus highlight) — unless the content is genuinely too short to justify it.
- [ ] Zero `<script>` tags, zero inline event handlers, zero external JS.
- [ ] `:focus-visible` outlines are present on every interactive element (lime ring, does not count toward the lime budget).
- [ ] `@media print` rules expand every `<details>` and reveal every tab panel.
- [ ] TL;DR, hero, mode-specific signature block, and footer footnote are never hidden behind interactive controls.
- [ ] Unknowns are labeled, not invented.
- [ ] Footer footnote is present with the `Editorial Obsidian — cs-html-explainer` attribution.
