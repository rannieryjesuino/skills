# Example prompt — ADR / DRE HTML (editorial)

User request:

> Gera uma DRE editorial em HTML pra essa decisão: a gente vai trocar o fluxo síncrono `Slack → API Gateway → Lambda → Bedrock Agent` por um fluxo assíncrono `Slack → API Gateway → Receiver Lambda → SQS → Consumer Lambda → Bedrock Agent → Slack Web API`. Pode ser com cara de revista, respiro grande, Playfair na manchete.

## Expected behavior

- Use `architecture-decision` mode (see `templates/architecture-decision.html`).
- Produce a single self-contained `.html` file.
- Apply the **Editorial Obsidian** visual identity (see `design/editorial-obsidian.md`).
- Hero is the **single-column variant** with an `.eyebrow-row` (`kicker · hairline · id`) and the 4-column `.hero-meta` ledger below the H1. The H1 carries exactly ONE italicized lime keyword — for example: `Decidimos <em>migrar</em> o fluxo para um pipeline assíncrono.`
- After the hero, render the page as the signature **two-page editorial spread**: `.col-left` carries the narrative (TL;DR, Contexto, Forças, Decisão, Por que venceu, Antes/Depois). `.col-right` carries the apparatus (Opções, Trade-offs, Consequências, Riscos, Impacto, ADR markdown, Próximos passos).
- The options list is a **vertical** `.options` stack (NOT a 4-column table). Render three options:
  - I — `Manter o fluxo síncrono e expandir timeouts` → `.option` with `.opt-tag.discard "Descartada"`.
  - II — `Pipeline assíncrono com SQS` → `.option.chosen` with `.opt-tag.chosen "Escolhida"`. The `.opt-title` is rendered in italic lime.
  - III — `SNS fan-out direto para os consumidores` → `.option` with `.opt-tag.discard "Descartada"`.
- The **Antes / Depois** block is a vertical stack with two `.panel` blocks separated by a typographic `.arrow-band` reading `DECISÃO TOMADA` (lime `↓` glyph before the label).
- Include an optional ADR source block inside `<pre class="code">` (compact ADR markdown, ≤ 60 lines).
- Mark unknown metrics (latência média, throughput de pico, custo mensal) as `A confirmar` — do not invent numbers.

## Suggested page outline

### Page A — narrative (left column)

1. `.folio-tag` — `Página A · Narrativa`.
2. `.tldr-paper` — single inversion of the page.
3. Contexto (prose, optional `.dropcap` on the first paragraph).
4. Forças em conflito (`.forces` with two `.force` blocks).
5. Decisão (single paragraph prose).
6. Por que esta opção venceu (`.checklist` with `.done` glyphs).
7. Antes / Depois (`.ba` stacked panels with `DECISÃO TOMADA` arrow band).

### Page B — apparatus (right column)

8. `.folio-tag` — `Página B · Aparato técnico`.
9. Opções consideradas (`.options` vertical stack, mandatory).
10. Trade-offs (`.forces` with two `.force` blocks).
11. Consequências positivas (`.checklist` with `.done`).
12. Consequências negativas (`.checklist` without `.done`).
13. Riscos e mitigações (list of `.risk` blocks with `.status danger/warning/success`).
14. Sistemas / times impactados (`table.editorial`).
15. ADR (`<pre class="code">`, optional).
16. Próximos passos (`.checklist`).

## Status badge

`ACEITO` rendered in the hero-meta ledger as `<span class="v lime">● Aceito</span>`. Never pill-shaped.

## Lime budget for this page

1. The H1 `<em>` (the decision verb).
2. The `.cta` primary in the masthead.
3. The `.eyebrow-row .kicker`.
4. The `.option.chosen .opt-title` (italic lime).
5. The `.panel.after .tag` ("Depois") OR a single inline link to the source ADR in a wiki — pick one.

Forbidden in this page:

- Lime as a background on any of the option rows.
- Lime inside the trade-offs or consequences checklists.
- Two italic-lime keywords in the same heading.

## Footnote attribution

Footer footnote left side stays fixed:

```
Editorial Obsidian — cs-html-explainer
```

The right side reads `Folio 02 / 05 · ADR / DRE`.
