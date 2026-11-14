---
version: 2
name: cross-cutting-specialized-block-components-import-inte-ae55
description: "Adoption of @openstatus/ui Component Library: Specialized Block Components Import Internal Type"
metadata:
  actual:
    adr_id: ae558f8e-06e7-4aa6-b8ce-9443bc6d77e4
    status: accepted
    candidate_group_id: 9ee8de6e-eefa-477b-b480-83f4f7bb50f2
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui Component Library: Specialized Block Components Import Internal Type

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** MAY: Specialized block components MAY import internal type definitions and localization helpers provided by sibling block modules within the shared interface hierarchy.

### Verify

```bash
# Discover the project dependency manifest and verification scripts, then execute workspace static analysis, linting, and type checking suites
```

**Accept when:**
- Static analysis validates that all composite interface primitives are imported directly from @openstatus/ui without unapproved direct third-party UI dependencies.
- The project type checking and component verification suites execute without errors across all consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>