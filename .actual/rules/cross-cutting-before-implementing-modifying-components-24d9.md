---
version: 2
name: cross-cutting-before-implementing-modifying-components-24d9
description: "Adoption of @openstatus/ui Component Library: Before Implementing Modifying Components That Consume"
metadata:
  actual:
    adr_id: 24d941e6-5586-48fd-91f8-785fac775d1d
    status: accepted
    candidate_group_id: 9ee8de6e-eefa-477b-b480-83f4f7bb50f2
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui Component Library: Before Implementing Modifying Components That Consume

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Before implementing or modifying components that consume @openstatus/ui, developers MUST inspect the repository dependency lock artifact to determine the exact resolved dependency version and verify API compatibility against that authoritative version.

### Verify

```bash
# Discover the project dependency manifest and verification scripts, then execute the workspace static analysis and linting suite to verify component import conformance.
# Discover and run the project type checker across all interface packages to validate component property and interface contract compliance.
```

**Accept when:**
- Static analysis validates that all composite interface primitives are imported directly from @openstatus/ui without unapproved direct third-party UI dependencies.
- The project type checking and component verification suites execute without errors across all consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, linting checks, and peer code review.
</enforcement>