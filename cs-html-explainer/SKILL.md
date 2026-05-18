---
name: cs-html-explainer
description: Use when the user wants an editorial dark-mode HTML artifact (huge serif headlines, generous whitespace, deep-teal surfaces, lime accent) for system explainers, ADR/DRE pages, implementation plans, incident reports, module maps, or executive-style handoff docs. Generates a single self-contained HTML file with the "Editorial Obsidian" identity — magazine-style respiro, Playfair Display + Geist typography, hairline dividers, and a strict 5-occurrence lime budget per page.
version: 2.0.0
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
- Avoid animation unless it improves comprehension.

## Diagram rules

- Use pure HTML/CSS/SVG. No Mermaid, no D3, no React, no external libraries (unless the user explicitly requests one).
- Boxes and arrows. Arrows in `--accent-moss`, never lime.
- Short labels (max ~30 characters per node).
- Include a short prose paragraph below every diagram explaining what the diagram does NOT show (side effects, async fan-out, retries — whatever is omitted).
- Avoid overcrowding — break into multiple diagrams if you have more than ~6 nodes.

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
- [ ] Unknowns are labeled, not invented.
- [ ] Footer footnote is present with the `Editorial Obsidian — cs-html-explainer` attribution.
