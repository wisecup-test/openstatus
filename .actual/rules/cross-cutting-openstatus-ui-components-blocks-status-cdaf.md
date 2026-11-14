---
version: 2
name: cross-cutting-openstatus-ui-components-blocks-status-cdaf
description: "Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Components Consuming Internationalization Utilities Status Formatting"
metadata:
  actual:
    adr_id: cdaf9bc2-582c-4435-99d2-17aa652a298d
    status: accepted
    candidate_group_id: 7cc4380c-3c72-4aae-8af2-e61755dd7860
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Components Consuming Internationalization Utilities Status Formatting

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STAT-001** SHOULD: Components consuming internationalization utilities for status formatting SHOULD coordinate localized translation keys with the domain variants declared in @openstatus/ui/components/blocks/status.types.

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