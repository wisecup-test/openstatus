---
version: 2
name: cross-cutting-composite-block-components-not-duplicate-eccb
description: "Adoption of @openstatus/ui Component Library: Composite Block Components Not Duplicate Foundational"
metadata:
  actual:
    adr_id: eccbe03d-0129-42dd-8bf8-6efdf760da66
    status: accepted
    candidate_group_id: 9ee8de6e-eefa-477b-b480-83f4f7bb50f2
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui Component Library: Composite Block Components Not Duplicate Foundational

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** MUST_NOT: Composite block components MUST NOT duplicate foundational primitive elements or introduce inline replacement implementations for established @openstatus/ui components.

### Verify

```bash
# Discover the project dependency manifest and verification scripts, then execute the workspace static analysis and linting suite to verify component import conformance.
# Discover and run the project type checker across all interface packages to validate component property and interface contract compliance.
```

**Accept when:**
- Static analysis validates that all composite interface primitives are imported directly from @openstatus/ui without unapproved direct third-party UI dependencies.
- The project type checking and component verification suites execute without errors across all consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, linting, and peer code review of component import paths must validate compliance.
</enforcement>