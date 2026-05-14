---
name: html-explainer
description: Use when the user wants a self-contained HTML artifact for technical explainers, ADR/DRE pages, implementation plans, incident reports, module maps, or handoff docs instead of plain Markdown.
version: 1.1.0
license: MIT
metadata:
  tags: [html, documentation, explainer, adr, dre, handoff]
---

# HTML Explainer Skill

## Overview

Use this skill to transform technical context into a polished, self-contained HTML page optimized for human reading.

The goal is not to replace Markdown everywhere. Prefer Markdown for versioned source documentation, KB/RAG ingestion, PR diffs, notes, and simple reference docs. Use HTML when the user wants a visual, navigable, presentation-friendly artifact.

## Core output contract

When this skill is invoked, generate exactly one complete `.html` file unless the user explicitly asks for multiple files.

The HTML must be:

- self-contained;
- readable offline;
- dark-mode first;
- responsive;
- accessible enough for internal documentation;
- visually structured with sections, cards, badges, tables, diagrams, timelines, and checklists when useful;
- written for humans who need to understand a system or decision quickly.

Do not use external CDNs, external fonts, external images, remote CSS, or remote JavaScript unless the user explicitly asks for them.

## Default visual style

Use the `Verdant Obsidian — Graphite Variant` design system.

General feel:

- graphite and near-black backgrounds;
- green used as an accent, not as the dominant background;
- clean developer-docs aesthetic;
- subtle borders;
- clear typographic hierarchy;
- restrained glow;
- elegant contrast;
- no excessive neon.

The page should feel like a premium internal technical document, not a gaming UI.

Reference the design tokens in `design/verdant-obsidian-graphite.md`.

## When to Use

Use when the user asks for any of the following (in any language):

- a visual HTML page explaining a system, flow, or integration;
- a human-friendly version of an ADR or architecture decision (a DRE);
- a visual implementation/migration/rollout plan;
- a visual incident report or postmortem;
- a module map, onboarding page, or codebase walkthrough;
- a handoff document for another team or engineer;
- a "prettier" or "more visual" version of existing technical documentation;
- a self-contained, shareable HTML artifact instead of plain Markdown.

Do not use this skill automatically when the user only wants a short answer, a normal Markdown doc, a code snippet, a README, or text for a ticket.

## Supported document modes

Pick the closest mode from the list below.

### 1. `system-explainer`

Use for explaining how a system, flow, integration, service, or feature works.

Recommended sections:

1. TL;DR
2. What problem this solves
3. High-level flow
4. Components
5. Data/messages involved
6. Sequence or architecture diagram
7. Operational details
8. Failure modes
9. Troubleshooting
10. FAQ
11. Next steps

Use template: `templates/system-explainer.html`

### 2. `architecture-decision`

Use for architecture decisions, ADRs, DREs, trade-off analysis, and decision explanation.

Generate a visual DRE by default. If the user asks for source documentation too, also include a compact ADR Markdown block inside the HTML as a copyable section.

Recommended sections:

1. TL;DR of the decision
2. Status
3. Context
4. Problem / forces in conflict
5. Options considered
6. Decision
7. Why this decision won
8. Trade-offs
9. Positive consequences
10. Negative consequences
11. Risks and mitigations
12. Impacted systems/teams
13. Before/after diagram
14. Follow-up actions

Use template: `templates/architecture-decision.html`

Definitions:

- ADR: Architecture Decision Record, optimized for long-term versioned record keeping.
- DRE: Decision Record Explainer, a visual and more human-friendly HTML explanation of the same decision.

If the user says "ADR/DRE", treat it as: produce an HTML DRE that preserves the core ADR facts.

### 3. `implementation-plan`

Use for plans, migrations, refactors, rollout plans, and technical delivery strategy.

Recommended sections:

1. Goal
2. Current state
3. Target state
4. Scope
5. Non-goals
6. Milestones
7. Step-by-step plan
8. Risks
9. Rollback
10. Validation checklist
11. Owners
12. Timeline

Use template: `templates/implementation-plan.html`

### 4. `incident-report`

Use for incidents, postmortems, debugging narratives, failure analysis, and operational reports.

Recommended sections:

1. Summary
2. Impact
3. Timeline
4. Root cause
5. Detection
6. What went well
7. What went wrong
8. Corrective actions
9. Preventive actions
10. Open questions

Use template: `templates/incident-report.html`

### 5. `module-map`

Use for codebase/module explanation, service ownership maps, dependency maps, and onboarding pages.

Recommended sections:

1. Module overview
2. Entry points
3. Key files
4. Main abstractions
5. Dependencies
6. Data flow
7. Extension points
8. Common changes
9. Risks
10. Testing strategy

Use template: `templates/module-map.html`

## Mandatory page structure

Every generated HTML artifact should include:

- a fixed or sticky sidebar when the page is large;
- a top title area;
- metadata chips when known: owner, date, status, version, system, environment;
- a TL;DR card near the top;
- navigable sections;
- at least one visual structure when useful:
  - flow diagram;
  - timeline;
  - option comparison table;
  - architecture boxes;
  - before/after panel;
  - checklist;
  - risk matrix.

For small documents, a compact layout is acceptable.

## Accessibility and readability rules

- Normal text should target at least WCAG AA contrast.
- Avoid long paragraphs.
- Prefer short cards, tables, and section summaries.
- Do not encode meaning with color alone; pair color with labels or icons.
- Keep font size readable.
- Use semantic HTML: `main`, `nav`, `section`, `article`, `header`, `table`, `button`.
- Provide visible focus states for interactive elements.
- Avoid animation unless it improves comprehension.

## Diagram rules

Prefer pure HTML/CSS/SVG diagrams.

Do not depend on Mermaid, D3, React, or external libraries unless the user explicitly requests them.

For diagrams:

- use boxes and arrows;
- keep labels short;
- include a short explanation below;
- avoid overcrowding;
- include "before/after" diagrams for decisions and migrations when possible.

## Content rules

Be honest about uncertainty.

If information is missing:

- mark it as "Unknown", "Assumption", or "Needs confirmation";
- do not invent owners, dates, environments, metrics, ARNs, URLs, resource names, team names, or API behavior;
- preserve ambiguity instead of filling gaps with fake confidence.

When using user-provided context, do not add external facts unless asked.

## ADR/DRE-specific rules

For architecture decisions:

- document only architecturally significant decisions;
- include alternatives that were considered;
- show why the chosen option won;
- include trade-offs and consequences;
- keep the source-of-truth ADR concise;
- use the HTML DRE as the human-friendly presentation layer.

Recommended status values:

- Proposed
- Accepted
- Superseded
- Deprecated
- Rejected

If a decision is superseded, include a link or placeholder to the newer decision when known.

## Output quality checklist

Before finishing, verify:

- The HTML opens as a standalone file.
- The design uses graphite/dark backgrounds, not green backgrounds.
- Green is used as accent only.
- All major sections are represented in the sidebar.
- TL;DR exists.
- Unknowns are clearly marked.
- There are no fake citations, fake dates, fake owners, fake resource IDs, or fake metrics.
- The HTML is easy to skim in under 60 seconds.
- The artifact is useful to share via chat, issue trackers, wikis, or a technical handoff.

## Common Pitfalls

1. **Using HTML when Markdown would be better.**
   If the user only needs source docs, KB text, PR-friendly diffs, or a short note, stay in Markdown.

2. **Shipping non-self-contained pages.**
   Do not pull fonts, CSS, JS, or diagrams from CDNs unless the user explicitly asks for that dependency.

3. **Inventing missing facts.**
   Unknown owners, dates, metrics, ARNs, and system names must stay marked as unknown or assumption.

4. **Overdoing the green.**
   Verdant Obsidian is graphite-first. Green is accent, not wall paint.

5. **Making pretty sludge.**
   If the page is hard to skim in under a minute, the artifact failed even if it looks fancy.

## Verification Checklist

- [ ] Output is exactly one standalone `.html` file unless the user asked for more
- [ ] No external CDN/font/JS dependency was added without explicit request
- [ ] TL;DR appears near the top
- [ ] Sidebar or equivalent navigation exists when the page is large
- [ ] At least one useful visual structure exists where appropriate
- [ ] Unknowns are labeled instead of invented
- [ ] Verdant Obsidian stays graphite-first with green as accent only
- [ ] The artifact is skimmable and readable offline
