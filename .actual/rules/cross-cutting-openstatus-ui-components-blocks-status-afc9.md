---
version: 2
name: cross-cutting-openstatus-ui-components-blocks-status-afc9
description: "Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Before Implementing Modifying Components That Consume"
metadata:
  actual:
    adr_id: afc91934-2a4e-4f0e-8a0b-b347299acc34
    status: accepted
    candidate_group_id: 7cc4380c-3c72-4aae-8af2-e61755dd7860
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Before Implementing Modifying Components That Consume

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status block visual components within the UI package, and utility and internationalization helper modules operating on status data.

### Rules

- **R-STATUS-001** MUST: Before implementing or modifying components that consume internal domain type modules or external date utilities, the consumer MUST inspect the repository lock artifact to determine exact resolved dependency versions.
- **R-STATUS-002** MUST: Ensure that modules consuming @openstatus/ui/components/blocks/status.types use type-only imports where possible to optimize compilation and prevent runtime bundle bloat.
- **R-STATUS-003** MUST: Coordinate date and timestamp representations defined in the domain contracts with centralized date utility functions to guarantee uniform temporal formatting.

### Verify

```bash
find . -type f \( -name "*.ts" -o -name "*.tsx" \) -exec grep -l "@openstatus/ui/components/blocks/status.types" {} +
echo "Discover repository build configuration and execute the static type verification suite to ensure contract compliance across status blocks"
```

**Accept when:**
- Static type analysis passes with zero type errors across all status block components and utility modules.
- All status block components source shared domain models exclusively from @openstatus/ui/components/blocks/status.types with no duplicate local domain interfaces.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>