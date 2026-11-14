---
version: 2
name: cross-cutting-openstatus-ui-components-blocks-status-2927
description: "Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Status Block Components Associated Utility Modules"
metadata:
  actual:
    adr_id: 2927bcef-3d82-42a3-b59d-8ceea6e10022
    status: accepted
    candidate_group_id: 7cc4380c-3c72-4aae-8af2-e61755dd7860
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Status Block Components Associated Utility Modules

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for status block visual components, utility modules, and internationalization helper modules operating on status data within the UI package.

### Rules

- **R-STATUS-001** MUST: Status block components and associated utility modules MUST import their domain type contracts directly from the internal module @openstatus/ui/components/blocks/status.types.

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