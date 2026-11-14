---
version: 2
name: cross-cutting-openstatus-ui-components-blocks-status-e2ce
description: "Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Internal Module Openstatus Components Blocks Status"
metadata:
  actual:
    adr_id: e2cee76f-b888-40d5-b05c-b9b5eb86fc89
    status: accepted
    candidate_group_id: 7cc4380c-3c72-4aae-8af2-e61755dd7860
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Internal Module Openstatus Components Blocks Status

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status monitoring block components, utilities, and internationalization helper modules operating on status data within the UI package.

### Rules

- **R-OPENSTATUS-001** MUST_NOT: The internal module @openstatus/ui/components/blocks/status.types MUST_NOT import visual presentation components or client UI elements to prevent circular dependencies.

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