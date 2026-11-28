# Domain Docs

This repository uses a single-context domain layout.

## Before exploring

- Read `GLOSSARY.md` at the repository root when it exists.
- Read ADRs in `docs/adr/` that touch the area being changed.
- If these files do not exist, continue without creating them merely to satisfy this guide. Create them when domain terms or durable architectural decisions are actually established.

## Layout

```text
/
├── GLOSSARY.md
├── docs/adr/
└── src/
```

Use glossary terms consistently in issue titles, domain models, API names, tests, and refactor proposals. If a change conflicts with an ADR, surface the conflict explicitly and propose whether the decision should be reopened.
