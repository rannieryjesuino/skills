# html-explainer Skill

An agent skill for generating self-contained, human-friendly HTML documentation artifacts.

## What it is for

Use this skill when you want an HTML page instead of Markdown for:

- system explainers;
- architecture decisions / ADR / DRE;
- implementation plans;
- incident reports;
- module maps;
- technical handoffs.

## What it is not for

Do not use it as the default format for all docs.

Markdown is still better for:

- versioned source documentation;
- small README files;
- KB/RAG ingestion;
- PR diffs;
- simple notes;
- docs that need clean text review.

## Visual identity

Default style: `Verdant Obsidian — Graphite Variant`.

Graphite/near-black backgrounds, green accent, readable contrast, technical premium feel.

## Folder structure

```text
html-explainer/
  SKILL.md
  README.md
  design/
    verdant-obsidian-graphite.md
  templates/
    base-page.html
    architecture-decision.html
    system-explainer.html
    incident-report.html
    implementation-plan.html
    module-map.html
  examples/
    adr-to-html-example.md
```

## Install

Drop the `html-explainer/` folder into wherever your agent loads skills from. Common locations:

```text
~/.claude/skills/html-explainer/
.claude/skills/html-explainer/
```

Keep the folder structure intact — `SKILL.md` references `design/`, `templates/`, and `examples/` by relative path.
