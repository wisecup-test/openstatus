---
version: 2
name: cross-cutting-interactive-components-requiring-client-371d
description: "Adoption of @openstatus/ui Component Library: Interactive Components Requiring Client Side State"
metadata:
  actual:
    adr_id: 371de14a-ea34-4329-b5b9-6ca8b1b87d66
    status: accepted
    candidate_group_id: 9ee8de6e-eefa-477b-b480-83f4f7bb50f2
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui Component Library: Interactive Components Requiring Client Side State

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** SHOULD: Interactive components requiring client-side state or event handling alongside @openstatus/ui primitives SHOULD declare explicit client component boundaries.
- **R-UI-002** MANDATORY: Newly authored block components MUST leverage existing primitives and utility functions from @openstatus/ui rather than defining separate styling helpers.
- **R-UI-003** MANDATORY: Interactive components consuming @openstatus/ui primitives and managing client-side events MUST reside within explicit client rendering boundaries.

### Verify

```bash
# Discover the project dependency manifest and verification scripts, then execute workspace static analysis, linting, and type checking suites
# (e.g., pnpm lint, pnpm check-types, or repository-specific validation scripts)
```

**Accept when:**
- Static analysis validates that all composite interface primitives are imported directly from @openstatus/ui without unapproved direct third-party UI dependencies.
- The project type checking and component verification suites execute without errors across all consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and linting checks configured in continuous integration pipelines enforce these requirements, and pull requests introducing duplicate primitive implementations or unapproved external UI libraries will fail validation.
</enforcement>