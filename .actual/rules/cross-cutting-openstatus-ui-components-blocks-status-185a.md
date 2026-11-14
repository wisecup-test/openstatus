---
version: 2
name: cross-cutting-openstatus-ui-components-blocks-status-185a
description: "Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Status Block Components Not Define Local"
metadata:
  actual:
    adr_id: 185a86b5-83a4-49d0-993a-a2feb5fd15e8
    status: accepted
    candidate_group_id: 7cc4380c-3c72-4aae-8af2-e61755dd7860
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Status Block Components Not Define Local

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STATUS-001** MUST_NOT: Status block components MUST_NOT define local duplicate interfaces or inline domain types for entities represented within @openstatus/ui/components/blocks/status.types.

### Verify

```bash
find . -type f \( -name "*.ts" -o -name "*.tsx" \) -exec grep -l "@openstatus/ui/components/blocks/status.types" {} +
```

**Accept when:**
- Static type analysis passes with zero type errors across all status block components and utility modules.
- All status block components source shared domain models exclusively from @openstatus/ui/components/blocks/status.types with no duplicate local domain interfaces.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>