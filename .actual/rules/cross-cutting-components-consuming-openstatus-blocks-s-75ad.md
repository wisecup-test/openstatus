---
version: 2
name: cross-cutting-components-consuming-openstatus-blocks-s-75ad
description: "Adoption of @openstatus/ui/components/blocks/status.utils Module: Components Consuming Openstatus Blocks Status Utils"
metadata:
  actual:
    adr_id: 75ad0c54-7934-4676-8b0a-a3c5568dbf43
    status: accepted
    candidate_group_id: 459b23c3-ed82-460d-a6fd-c36d98a0ae9e
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status.utils Module: Components Consuming Openstatus Blocks Status Utils

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for status visualization and event presentation components requiring status formatting, calculation, or shared transformation helpers, and UI block components rendering status indicators, events, or metric timelines.

### Rules

- **R-ST-001** SHOULD: Components consuming @openstatus/ui/components/blocks/status.utils SHOULD co-locate shared data structures via companion type definitions rather than defining divergent local models.

### Verify

```bash
# Discover and execute static analysis and linting command from the project root
npm run lint || yarn lint || pnpm lint

# Discover and execute test suite command
npm test || yarn test || pnpm test

# Discover and execute type checker command
npm run typecheck || yarn typecheck || pnpm typecheck
```

**Accept when:**
- Static analysis and linting pass with zero import boundary or module resolution errors across status block components.
- All unit and integration tests for status utilities and consuming status components pass without failure.
- The type checker completes with zero errors confirming strict interface compliance across utility call sites.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute repository linting, type validation, and unit tests on every pull request.
</enforcement>