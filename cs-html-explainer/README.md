# cs-html-explainer

A Claude Code skill that generates self-contained, editorial dark-mode HTML pages for technical artifacts: ADRs/DREs, implementation plans, incident reports, system explainers, and module maps.

Sibling of `html-explainer` — same intent, different visual identity:

- `html-explainer` — graphite developer-docs feel, neon green accent, Inter font.
- `cs-html-explainer` — editorial magazine feel, lime green accent, Playfair Display + Geist, deep teal surfaces.

## Visual identity — "Editorial Obsidian"

- Warm near-black background (`#1A1615`), never pure black.
- Deep teal card surfaces (`#243833`) for elevation.
- Lime green accent (`#DBFFA5`) used with extreme discipline — max 5 occurrences per page.
- Huge Playfair Display headlines with ONE italicized keyword in lime per H1 (the "marca-texto").
- Generous whitespace — 60–70% of the page is empty by design.
- Off-white paper (`#FCF8EC`) used once per page maximum, as an editorial pull-quote.
- Status signaling via warm tones + uppercase typographic kickers. No red/yellow UI palette.

## Install

Drop the folder under your Claude Code skills directory. Verify with:

```sh
ls ~/.claude/skills/cs-html-explainer/SKILL.md
```

Trigger phrases:

- "gera um HTML editorial pra esse ADR"
- "uma DRE com cara de revista pra essa decisão"
- "página deck-style pra esse postmortem"
- "explica esse módulo com respiro, headlines Playfair"

If the user does not ask for an editorial / magazine / deck look, prefer the sibling `html-explainer` skill.

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
    └── adr-to-html-example.md            # narrative example
```

`base-page.html` carries the full CSS and HTML scaffold. The 5 mode files are short comment-only documents describing the deltas for each document type — they reference `base-page.html` as the source of truth.

## Hard rules (read before contributing)

- The skill output must NEVER reference a source brand, company, or product. The footer is fixed to `Editorial Obsidian`.
- Every H1 has exactly ONE italicized lime keyword. Not zero. Not two.
- Lime is never a background larger than the CTA pill.
- No red / yellow / orange anywhere in the chrome.
- Body is always left-aligned. Centering is permitted only for the incident-report hero.
- Page must remain legible offline if Google Fonts is blocked (fallback to Georgia + system-ui).

See `SKILL.md` for the complete rule set and the verification checklist.
