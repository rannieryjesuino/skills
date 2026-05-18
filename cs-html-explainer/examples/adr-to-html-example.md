# Example prompt: ADR / DRE HTML (editorial)

User request:

> Gera uma DRE editorial em HTML pra essa decisão: a gente vai trocar o fluxo síncrono `Slack → API Gateway → Lambda → Bedrock Agent` por um fluxo assíncrono `Slack → API Gateway → Receiver Lambda → SQS → Consumer Lambda → Bedrock Agent → Slack Web API`. Pode ser com cara de revista, respiro grande, Playfair na manchete.

Expected behavior:

- Use `architecture-decision` mode.
- Produce a single self-contained HTML file.
- Apply the **Editorial Obsidian** visual identity (see `design/editorial-obsidian.md`).
- Hero H1 must contain ONE italicized lime keyword — for example: `Decidimos <em>migrar</em> o fluxo para um pipeline assíncrono.`
- Include a full-width option comparison table (Síncrono atual vs Assíncrono proposto vs SNS fan-out alternative — hairlines, no zebra, chosen row marked with `.status.success` "ESCOLHIDA").
- Include the stacked **Antes / Depois** panel with a hairline label `→ DEPOIS` between two teal cards.
- Include an optional ADR source block inside `<pre class="code">` (compact ADR markdown, ≤ 60 lines).
- Mark unknown metrics (latência média, throughput de pico, custo mensal) as `A confirmar` — do not invent numbers.

Suggested sections (in order):

1. Hero (kicker `ADR / DRE` + Playfair H1 with one italicized lime verb).
2. TL;DR (`.card.invert` off-white).
3. Contexto.
4. Problema / forças em conflito.
5. Opções consideradas (comparison table — mandatory).
6. Decisão.
7. Por que esta opção venceu.
8. Trade-offs (.grid.two of teal cards).
9. Consequências positivas (.checklist with .done glyphs).
10. Consequências negativas (.checklist, no .done).
11. Riscos e mitigações (.grid.three with status danger / warning / success).
12. Sistemas e times impactados (table).
13. Antes / Depois (mandatory stacked panel).
14. ADR source (optional `<pre class="code">`).
15. Próximos passos (.checklist).

Status badge: `ACEITO` uppercase Geist 11px in the metadata row — never pill-shaped.

Lime budget for this page:

- 1 in H1 `<em>` (the decision verb).
- 1 CTA pill in topbar.
- 1 active nav left-border.
- 1 in the "ESCOLHIDA" success label inside the comparison table.
- ≤ 1 inline link (e.g., to the source ADR in a wiki).

Footer attribution stays fixed: `Gerado por cs-html-explainer · Editorial Obsidian`.
