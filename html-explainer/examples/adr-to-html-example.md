# Example prompt: ADR/DRE HTML

User request:

> Gere um HTML DRE explicando a decisão de trocar um fluxo síncrono Slack → API Gateway → Lambda → Bedrock Agent por um fluxo assíncrono Slack → API Gateway → Receiver Lambda → SQS → Consumer Lambda → Bedrock Agent → Slack Web API.

Expected behavior:

- Use `architecture-decision` mode.
- Produce a self-contained HTML page.
- Use Verdant Obsidian — Graphite Variant.
- Include a compact ADR copy block.
- Include before/after diagrams.
- Include trade-offs and consequences.
- Mark unknown metrics as unknown instead of inventing values.

Suggested sections:

1. TL;DR
2. Status: Accepted
3. Context
4. Forces in conflict
5. Options considered
6. Decision
7. Why this won
8. Before vs after
9. Consequences
10. Risks and mitigations
11. Rollout checklist
12. Compact ADR source
