---
version: 2
name: cross-cutting-before-implementing-modifying-code-that-bc5b
description: "Adoption of @openstatus/emails Module for Internal Email Dispatch: Before Implementing Modifying Code That Consumes"
metadata:
  actual:
    adr_id: bc5b9fd6-7739-4765-ae12-533d73d9b431
    status: accepted
    candidate_group_id: e7fd5b3d-c86c-4a8d-8a14-d0b1f3338248
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails Module for Internal Email Dispatch: Before Implementing Modifying Code That Consumes

Scope: applies to `apps/web/src/app/api/internal/email/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Before implementing or modifying code that consumes @openstatus/emails, the consumer MUST inspect the repository lock or resolution artifact to determine the exact resolved dependency versions.

### Verify

```bash
# Discover and execute project verification and workspace type check for the email module and route handlers
```

**Accept when:**
- All internal email route handlers import email generation and dispatch routines exclusively from the @openstatus/emails library.
- Workspace type checking and project verification test suites pass without errors across all internal email dispatch endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>