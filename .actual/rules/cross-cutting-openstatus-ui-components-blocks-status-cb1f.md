---
version: 2
name: cross-cutting-openstatus-ui-components-blocks-status-cb1f
description: "Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Utility Modules Operating Status Domain Objects"
metadata:
  actual:
    adr_id: cb1fd0cf-60e6-4e08-9791-2cb7ace80735
    status: accepted
    candidate_group_id: 7cc4380c-3c72-4aae-8af2-e61755dd7860
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Internal Module Adoption: @openstatus/ui/components/blocks/status.types Domain Contract: Utility Modules Operating Status Domain Objects

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STATUS-001** SHOULD: Utility modules operating on status domain objects SHOULD accept inputs typed strictly against `@openstatus/ui/components/blocks/status.types` contracts and return standardized status representations.
- **R-STATUS-002** MANDATORY: Modules consuming `@openstatus/ui/components/blocks/status.types` MUST use type-only imports where possible to optimize compilation and prevent runtime bundle bloat.
- **R-STATUS-003** MANDATORY: Date and timestamp representations defined in the domain contracts MUST be coordinated with centralized date utility functions to guarantee uniform temporal formatting.

### Verify

```bash
find . -type f \( -name "*.ts" -o -name "*.tsx" \) -exec grep -l "@openstatus/ui/components/blocks/status.types" {} +
# Discover repository build configuration and execute the static type verification suite to ensure contract compliance across status blocks
```

**Accept when:**
- Static type analysis passes with zero type errors across all status block components and utility modules.
- All status block components source shared domain models exclusively from `@openstatus/ui/components/blocks/status.types` with no duplicate local domain interfaces.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static type verification suites and peer code reviews enforce compliance.
</enforcement>