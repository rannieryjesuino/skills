---
name: cs-html-explainer
description: Use when the user wants an editorial dark-mode HTML artifact (huge serif headlines, generous whitespace, deep-teal surfaces, lime accent) for system explainers, ADR/DRE pages, implementation plans, incident reports, module maps, or executive-style handoff docs. Choose this over html-explainer when the user asks for a more editorial, magazine-style, deck-style, or "respiro" visual.
version: 1.0.0
license: MIT
metadata:
  tags: [html, documentation, explainer, adr, dre, handoff, editorial, deck]
---

# Editorial HTML Explainer Skill

## Overview

Use this skill to transform technical context into a single self-contained HTML page styled like an editorial magazine spread — huge Playfair Display headlines, generous whitespace, deep-teal surfaces, and a single disciplined lime-green accent.

Prefer Markdown for source documentation, KB ingestion, PR diffs, and short notes. Use this skill when the user wants a polished, navigable artifact that looks more like a deck page than a developer dashboard.

This skill is a sibling of `html-explainer`. They share intent (one self-contained HTML for technical content) but differ in visual identity:

- `html-explainer` — graphite developer-docs feel, neon green accent, Inter font.
- `cs-html-explainer` — editorial magazine feel, lime green accent, Playfair + Geist, deep teal surfaces.

If the user does not specifically ask for an editorial / magazine / deck / serif look, prefer `html-explainer`.

## Core output contract

When this skill is invoked, generate exactly one complete `.html` file unless the user explicitly asks for multiple files.

The HTML must be:

- self-contained for offline reading (the body, layout, and components must render correctly with no network);
- dark-mode (warm near-black `#1A1615`, never pure `#000`);
- responsive (sidebar collapses at 980px);
- accessible (WCAG AA contrast);
- structured with sections, hairlines, cards, tables, diagrams, timelines, and checklists when useful;
- written for humans who need to grasp a system or decision quickly.

### Font loading exception (delta from html-explainer)

Google Fonts CDN is allowed — and recommended — for loading Playfair Display, Geist, and Geist Mono. The page must remain legible and on-brand offline via the fallback stack (Georgia, system-ui, SFMono). Use the `<link>` block documented in `design/editorial-obsidian.md`. Do not load any other external resource (images, CSS, JS, icons).

## Default visual style — Editorial Obsidian

See `design/editorial-obsidian.md` for the full token set and usage rules.

Quick summary:

- Page background: `--bg` `#1A1615` (warm near-black).
- Card surfaces: `--surface` `#243833` (deep teal).
- Accent: `--accent-lime` `#DBFFA5`, used in at most 5 places per page.
- Inversion: `--surface-inverted` `#FCF8EC` (paper) — once per page maximum.
- Forest green `--surface-forest` `#0F352B`: only for the incident-report root-cause card.
- Pure black `#000000`: only for the danger status bar, never as a page background.
- Typography: Playfair Display for headlines, Geist for body/UI, Geist Mono for code.

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

Pick the closest mode from the list below.

### 1. `system-explainer`

How a system, flow, integration, service, or feature works.

Recommended sections: TL;DR · Problema · Fluxo · Componentes · Dados · Arquitetura · Operação · Falhas · Troubleshooting · FAQ · Próximos passos.

Template: `templates/system-explainer.html`.

Mode-specific element: horizontal `.flow` diagram between hero and details.

### 2. `architecture-decision`

ADRs, DREs, trade-off analysis, decision explanation. Generates a visual DRE; if the user asks for a source ADR too, include a compact ADR Markdown block inside `<pre class="code">`.

Recommended sections: TL;DR · Status · Contexto · Problema · Opções · Decisão · Por que venceu · Trade-offs · Consequências (+/−) · Riscos · Impacto · Antes/Depois · Próximos passos.

Template: `templates/architecture-decision.html`.

Mode-specific elements: full-width option comparison table (hairlines, no zebra) and a stacked Antes/Depois panel separated by a hairline label "→ DEPOIS".

Status values: `PROPOSTO`, `ACEITO`, `SUPERADO`, `DEPRECIADO`, `REJEITADO` — always uppercase Geist, never pill-shaped.

### 3. `implementation-plan`

Migrations, refactors, rollout plans, technical delivery strategy.

Recommended sections: Objetivo · Estado atual · Estado-alvo · Escopo · Não-objetivos · Marcos · Plano passo-a-passo · Riscos · Rollback · Validação · Donos · Timeline.

Template: `templates/implementation-plan.html`.

Mode-specific elements: vertical timeline with lime square markers (8×8px) and a rollback block that uses the inversion exception (`.card.invert`).

### 4. `incident-report`

Postmortems, debugging narratives, failure analysis.

Recommended sections: Resumo · Impacto · Timeline · Causa raiz · Detecção · O que funcionou · O que falhou · Ações corretivas · Ações preventivas · Questões abertas.

Template: `templates/incident-report.html`.

Mode-specific elements: 3 oversized Playfair stat blocks for Impacto, a Geist-Mono-rail timeline, and a single forest-green card for Causa raiz. This is the only mode allowed to centralize the hero.

### 5. `module-map`

Codebase/module explanation, service ownership maps, dependency maps, onboarding pages.

Recommended sections: Visão geral · Entry points · Mapa de arquivos · Abstrações · Dependências · Fluxo de dados · Extensão · Mudanças comuns · Riscos · Testes.

Template: `templates/module-map.html`.

Mode-specific elements: oversized Playfair-italic-lime entry-point links and a 2-column file-tree-plus-annotations grid.

## Mandatory page structure

Every generated HTML artifact must include:

- a sticky sidebar (220px) with a Playfair-italic brand block and a section nav list;
- a sticky topbar (56px) with `Exportar / PDF` ghost button and `Copiar resumo` lime CTA;
- a hero with: kicker (uppercase Geist) · H1 Playfair with ONE `<em>` in lime · subtitle Geist Light · metadata row (Status · Owner · Atualizado, separated by `   |   `);
- a TL;DR card near the top (use `.card.invert` for the editorial pull-quote treatment);
- navigable sections separated by full-bleed hairlines (`--hairline-alpha`);
- at least one visual structure (flow, timeline, table, before/after, checklist, risk grid);
- a small Geist 12px footer: `Gerado por cs-html-explainer · Editorial Obsidian`.

## Editorial respiro rule

Apply the "60–70% of the page is empty" rule literally:

- Hero kicker → H1: 16px.
- H1 → subtitle: 32–40px.
- Subtitle → metadata row: 64px.
- Metadata row → first hairline: 120px.
- Between sections: 96px before hairline, 64px after.
- Section → footer: 120px.
- Max content width: 1200px.
- Body alignment: ALWAYS left. Centering is allowed only for the incident-report hero.

Use the spacing scale tokens (`--space-sm` through `--space-2xl`) — do not invent ad-hoc values.

## Lime discipline rule

`--accent-lime` `#DBFFA5` may appear in at most **5 places** on a single page:

1. ONE italicized keyword in the H1 (the marca-texto — never zero, never two).
2. The primary CTA pill in the topbar.
3. The active sidebar item (2px left border only — no fill).
4. Inline links (color + underline on hover).
5. The `SUCESSO` status label (text + `●` bullet glyph).

Forbidden uses of lime:

- Card backgrounds.
- Borders on multiple cards in the same section.
- Inside tables (headers, rows, cells).
- Two italicized lime keywords in the same heading.
- As a decorative gradient, glow, or shadow.

The big lime stat numbers in incident-report and the lime milestone markers in implementation-plan each count against this budget. When you have more than 2 of those, render the extras in `--text` white or `--accent-moss`.

## Status signaling rule

No traditional UI red/yellow. Status is paired color + uppercase typographic label. Mapping:

| Status | Color treatment | Label |
|---|---|---|
| Success / OK | `●` glyph in `--accent-lime`, label in lime | `SUCESSO`, `OK`, `PLANEJADO` |
| Warning / Attention | `▲` glyph in `--surface-inverted` (#FCF8EC), label in off-white | `ATENÇÃO` |
| Danger / High risk | Vertical 4×16px bar in `--pure-black`, label in white | `RISCO ALTO`, `BLOQUEADO`, `FALHA` |
| Neutral / Info | Uppercase label in `--text-meta`, no glyph | `NOTA` |

Color must always be paired with the typographic uppercase kicker. Never signal status with color alone.

## No source-brand reference rule

The skill output must NOT mention any source brand, company name, internal codename, or product that inspired this visual identity. The footer attribution is fixed to `Editorial Obsidian`. The brand block in the sidebar reads `Editorial` / `HTML Explainer`.

If the user provides company or product names in the content they want documented, those names are fine inside body text — but they must never appear in skill chrome (footer, sidebar brand, kickers, template comments visible to the reader).

## Accessibility and readability

- All body text must meet WCAG AA contrast on the relevant surface.
  - White `#FFFFFF` on `--surface` `#243833` -> ≈ 9.4 (AA pass).
  - Lime `#DBFFA5` on `--text-inverted` `#1A1615` (CTA) -> ≈ 14 (AAA pass).
  - Muted `#9E9E9E` on `--bg` `#1A1615` -> ≈ 5.2 (AA pass).
  - Meta `#666666` on `--bg` -> ≈ 3.0 — acceptable ONLY for non-essential metadata; NEVER for body text.
- Use semantic HTML: `main`, `nav`, `aside`, `section`, `article`, `header`, `table`, `button`, `details`/`summary`.
- Provide visible focus states (`:focus-visible { outline: 2px solid var(--accent-lime); outline-offset: 3px; }`).
- Pair color with text or icons — never color alone.
- Avoid animation unless it improves comprehension.

## Diagram rules

- Use pure HTML/CSS/SVG. No Mermaid, no D3, no React, no external libraries (unless the user explicitly requests one).
- Boxes and arrows. Arrows in `--accent-moss`, never lime.
- Short labels (max ~30 characters per node).
- Include a short prose explanation below every diagram.
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
- No source-brand name appears in chrome (footer, sidebar, kickers).
- TL;DR exists near the top.
- All major sections are represented in the sidebar nav.
- Unknowns are clearly marked, not invented.
- The artifact is skimmable in under 60 seconds.

## Common pitfalls

1. **Using lime as a background.** Lime is for one H1 keyword, the CTA pill, the active nav border, links, and the success label. Nothing else.
2. **Centering body text.** Always left-aligned. The only exception is the incident-report hero.
3. **Forgetting to italicize one keyword in H1.** Every page must have exactly one `<em>` in the H1. Not zero, not two.
4. **Using red or yellow UI status colors.** No `#FF6B86`, no `#E8BD66`. Use the warm-palette + uppercase label mapping.
5. **Mentioning the source brand.** The skill is generic. Footer is fixed to `Editorial Obsidian`.
6. **Crowding the hero.** Apply the respiro scale strictly (16 → 32 → 64 → 120 px gaps).
7. **Falling back to Inter.** Body font is Geist, with system-ui as fallback. Inter is the other skill's font.
8. **Two inverted cards on the same page.** The off-white inversion is a once-per-page editorial pull-quote. Use it for TL;DR or for the implementation-plan rollback block — not both.
9. **Lime gradients, glows, or shadows.** Editorial Obsidian is flat. Radius is 4px on cards, 999px only on the CTA pill.
10. **Skipping the kicker.** Every hero has a kicker uppercase Geist 11px above the H1. The kicker labels the mode (`POSTMORTEM`, `ADR / DRE`, etc).

## Verification checklist

- [ ] Output is exactly one standalone `.html` file unless the user asked for more.
- [ ] No external CDN/CSS/JS dependency was added beyond the Google Fonts `<link>`.
- [ ] Fonts degrade gracefully offline (Georgia / system-ui / SFMono fallbacks).
- [ ] TL;DR appears near the top.
- [ ] Sidebar nav exists with each major section listed.
- [ ] H1 contains exactly one italicized lime keyword.
- [ ] Lime usage ≤ 5 occurrences (manually counted).
- [ ] No red, yellow, or orange anywhere in the chrome.
- [ ] Status badges always pair color with uppercase Geist label.
- [ ] No source-brand reference in chrome (footer is `Editorial Obsidian`).
- [ ] Page background is `#1A1615`, never `#000`.
- [ ] At least one mode-specific visual element is present (flow, timeline, before/after, impact stats, file tree, or comparison table).
- [ ] Body is left-aligned everywhere except (optionally) the incident-report hero.
- [ ] Hairlines separate sections, not framed boxes.
- [ ] Unknowns are labeled, not invented.
