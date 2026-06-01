# Development Patterns

Reusable, provider-neutral development patterns and conventions — sanitized for public sharing.

## Contents

- [provider-neutral-knowledge-map.md](provider-neutral-knowledge-map.md) — Keep durable agent
  knowledge in a git-tracked, provider-neutral `AGENTS.md` (a small always-loaded router plus a
  load-on-demand Knowledge Map) rather than a single provider's machine-local memory.
- [version-rollback-with-kb-reconciliation.md](version-rollback-with-kb-reconciliation.md) — Roll
  back code + KB + journals + SQL together, then deliberately carry forward code-independent
  knowledge; revert databases by rules (down-migration / backup), prose by human-approved judgment.

## How to use

Reference these patterns when applicable; they are written to generalize across stacks.
