---
version: 2
name: cross-cutting-route-handlers-not-construct-raw-6a23
description: "Adoption of @openstatus/emails Module for Internal Email Dispatch: Route Handlers Not Construct Raw Html"
metadata:
  actual:
    adr_id: 6a23b453-3136-4672-9585-26fdbd675ddb
    status: accepted
    candidate_group_id: e7fd5b3d-c86c-4a8d-8a14-d0b1f3338248
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails Module for Internal Email Dispatch: Route Handlers Not Construct Raw Html

Scope: applies to `apps/web/src/app/api/internal/email/**`.

These rules are ALWAYS ACTIVE for internal API route handlers responsible for transactional email creation and dispatch, including backend server endpoints triggering team invitations, customer feedback notifications, and system emails.

### Rules

- **R-EMAIL-001** MUST_NOT: API route handlers MUST NOT construct raw HTML or template strings directly within HTTP transport handler routines.
- **R-EMAIL-002** MANDATORY: Discover the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact, look up official documentation for the exact version, confirm every API exists in that version, and re-run per dependency at point of use before writing code using a versioned library.
- **R-EMAIL-003** MANDATORY: Import email templates and delivery utilities directly from `@openstatus/emails` at the boundary of internal email dispatch endpoints.
- **R-EMAIL-004** MANDATORY: Ensure authorization verification executes prior to invoking email template rendering or delivery procedures.

### Verify

```bash
# Discover and execute the project verification task for the email module and consuming web route handlers
# Discover and execute the workspace type check and test suites covering internal email API routes
```

**Accept when:**
- All internal email route handlers import email generation and dispatch routines exclusively from the @openstatus/emails library.
- Workspace type checking and project verification test suites pass without errors across all internal email dispatch endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance is verified by automated workspace type checking, continuous integration verification suites, and peer code reviews during pull request inspection.
</enforcement>