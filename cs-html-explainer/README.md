# cs-html-explainer

A Claude Code skill that generates self-contained, editorial dark-mode HTML pages for technical artifacts: ADRs/DREs, implementation plans, incident reports, system explainers, and module maps.

The output reads like a magazine spread, not a developer dashboard.

## Visual identity — "Editorial Obsidian"

- Warm near-black background (`#1A1615`), never pure black.
- Deep teal panel surface (`#243833`) reserved for code blocks and dense apparatus.
- Lime green accent (`#DBFFA5`) used with extreme discipline — max **5 occurrences per page**.
- Huge Playfair Display headlines with **ONE italicized keyword in lime** per H1 (the "marca-texto").
- Generous whitespace — 60–70% of the page is empty by design.
- Off-white paper (`#FCF8EC`) for the TL;DR band and (in implementation plans) a full-bleed rollback pull-quote.
- Forest green (`#0F352B`) reserved for the incident-report root-cause band.
- Status signaling via warm tones + uppercase typographic kickers. No red/yellow UI palette.

## Chrome — what the page looks like at a glance

- A sticky **masthead** at the top: `Vol. 01 · Nº NN — {{MODE}}` on the left, the `Editorial Obsidian` wordmark in italic Playfair in the center, `Exportar / PDF` ghost button + `Copiar resumo` lime CTA on the right.
- A **hero** with one of three variants:
  - left-rail metadata + huge serif headline (system-explainer);
  - single column with an eyebrow-row of `kicker · hairline · id` + 4-column ledger (ADR / plan / module map);
  - centered `.hero.front` with a `Severidade` badge inline (incident-report only).
- A **lede row** below the hero (`Sinopse` marker + a 20–28px paragraph in `--text-muted`).
- A **TL;DR paper band** (the single off-white inversion of the page).
- A **mode-specific signature block** (see Modes below).
- Vertical **sections** in marginalia (number + topic + small `.ref` on the left, content on the right) separated by hairlines.
- A **footer footnote** (`Editorial Obsidian — cs-html-explainer · Folio NN / 05`).

There is no 220px sticky sidebar and no 56px topbar with right-aligned action buttons. The masthead replaces both.

## Modes

| Mode | Signature block |
|---|---|
| `system-explainer` | A horizontal `.flow` 5-column diagram with hairline gutters and italic-serif node names. |
| `architecture-decision` | A two-page editorial spread: left column narrative + right column apparatus. Options are a vertical stack with the chosen one in italic lime. Antes/Depois is a vertical stack with a `DECISÃO TOMADA` arrow band between panels. |
| `implementation-plan` | A horizontal **waves band**: typographic ruler with week ticks + three wave cards. The critical wave is lime; the rest stay white. The rollback section is a full-bleed `.paper-band` (paper inversion). |
| `incident-report` | Centered `.hero.front` with a severity badge. An impact band with three oversized Playfair stats (only the most severe is lime). A `.timeline-list` with three marker variants (default / critical / lime). A full-bleed `.forest-band` for the root cause. |
| `module-map` | An oversized **entry-points band** with italic-lime HTTP verbs / worker prefixes. An asymmetric body grid: a sticky file tree on the left + a wide content column on the right. |

## Trigger phrases

- "gera um HTML editorial pra esse ADR"
- "uma DRE com cara de revista pra essa decisão"
- "página deck-style pra esse postmortem"
- "explica esse módulo com respiro, headlines Playfair"
- "plano de migração em formato editorial"

## Install

Drop the folder under your Claude Code skills directory. Verify with:

```sh
ls ~/.claude/skills/cs-html-explainer/SKILL.md
```

The skill is invoked automatically when its description matches the user's request. You can also invoke it by name in conversation.

## Files

```
cs-html-explainer/
├── SKILL.md                              # main spec + rules
├── README.md                             # this file
├── design/
│   └── editorial-obsidian.md             # tokens + usage rules
├── templates/
│   ├── base-page.html                    # canonical scaffold (all CSS lives here)
│   ├── system-explainer.html             # mode notes: how a system works
│   ├── architecture-decision.html        # mode notes: ADR / DRE
│   ├── implementation-plan.html          # mode notes: rollout / migration
│   ├── incident-report.html              # mode notes: postmortem
│   └── module-map.html                   # mode notes: codebase walkthrough
└── examples/
    └── adr-to-html-example.md            # narrative example prompt
```

`base-page.html` carries the full CSS scaffold and the HTML skeleton (masthead, hero, lede, TL;DR, sections, footer). The 5 mode files are short comment-only documents describing the deltas for each document type and their signature blocks — they reference `base-page.html` as the source of truth.

## Hard rules (read before contributing)

- The skill output must NEVER reference a source brand, company, or product. The masthead wordmark and footer attribution are fixed to `Editorial Obsidian`.
- Every H1 has exactly ONE italicized lime keyword. Not zero. Not two.
- Lime is never a background larger than the CTA pill.
- No red / yellow / orange anywhere in the chrome.
- Body is always left-aligned. Centering is permitted only for the incident-report `.hero.front`.
- Page background is `#1A1615` — never pure `#000`.
- The page must remain legible offline if Google Fonts is blocked (fallback to Georgia + system-ui).
- The default container is hairlines and full-bleed bands. Framed cards (`.card` with a full border) are reserved for `<pre class="code">` and similar dense panels.

See `SKILL.md` for the complete rule set and the verification checklist.
