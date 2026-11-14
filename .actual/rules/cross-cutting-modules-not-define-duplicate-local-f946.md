---
version: 2
name: cross-cutting-modules-not-define-duplicate-local-f946
description: "Adoption of @openstatus/ui Status Block Components and Shared Types: Modules Not Define Duplicate Local Status"
metadata:
  actual:
    adr_id: f9461cd0-23fd-49c4-8eb9-4347b4a54e80
    status: accepted
    candidate_group_id: 6ffc73a3-906b-49a6-bf40-eca1e97f23d2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/ui Status Block Components and Shared Types: Modules Not Define Duplicate Local Status

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for status dashboard pages, incident timeline views, service history displays, and data aggregation functions that map telemetry and incident records into daily status intervals.

### Rules

- **R-OPENSTATUS-001** MUST_NOT: Modules MUST NOT define duplicate or local status severity, incident event, or daily status type declarations that duplicate contracts provided by @openstatus/ui.
- **R-OPENSTATUS-002** MANDATORY: The consumer MUST discover and execute the repository type-checking script across all workspace packages to ensure compliance with shared type contracts.
- **R-OPENSTATUS-003** MANDATORY: The consumer MUST discover and run the repository linter to detect unapproved local status type definitions or bypassed package imports.
- **R-OPENSTATUS-004** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for exact version, confirm every API/class/function exists in that version, re-run steps per dependency at point of use.

### Verify

```bash
# Discover and execute workspace type checking
npm run typecheck

# Discover and run the repository linter
npm run lint

# Execute automated test suite for status timeline components
npm test
```

**Accept when:**
- All components rendering status timelines import presentation blocks and type contracts exclusively from @openstatus/ui.
- Static type checking and automated test suites confirm zero type mismatches across status calculations and UI bindings.
- Workspace verification pipelines pass with zero unresolved dependencies or unregistered module imports.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing duplicate status domain types or localized status bar implementations fail automated validation, and reviewers require migration to the canonical @openstatus/ui components and type definitions before merging.
</enforcement>