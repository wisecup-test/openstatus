---
version: 2
name: cross-cutting-developers-inspect-repository-lock-artif-24c3
description: "Adoption of @openstatus/ui/components/blocks/status.utils Module: Developers Inspect Repository Lock Artifact Manifest"
metadata:
  actual:
    adr_id: 24c3b7cb-c911-49dc-856d-d2dee0ce2ed0
    status: accepted
    candidate_group_id: 459b23c3-ed82-460d-a6fd-c36d98a0ae9e
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status.utils Module: Developers Inspect Repository Lock Artifact Manifest

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status visualization and event presentation components requiring status formatting, calculation, or shared transformation helpers, and UI block components rendering status indicators, events, or metric timelines.

### Rules

- **R-STAT-001** MUST: Developers MUST inspect the repository lock artifact and manifest to ensure consistent resolution of shared workspace modules before modifying internal utility interfaces.

### Verify

```bash
# Discover and execute the project static analysis, linting, type-checking, and test commands from the root manifest
# Example discovery routine:
PACKAGE_MANAGER=$(which pnpm || which yarn || which npm)
$PACKAGE_MANAGER run lint
$PACKAGE_MANAGER run typecheck
$PACKAGE_MANAGER test
```

**Accept when:**
- Static analysis and linting pass with zero import boundary or module resolution errors across status block components.
- All unit and integration tests for status utilities and consuming status components pass without failure.
- The type checker completes with zero errors confirming strict interface compliance across utility call sites.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration and code reviews enforce module usage and verify that status logic is imported from the canonical utility module rather than reimplemented locally.
</enforcement>