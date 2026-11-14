---
version: 2
name: cross-cutting-internal-email-route-endpoints-verify-72e7
description: "Adoption of @openstatus/emails Module for Internal Email Dispatch: Internal Email Route Endpoints Verify Caller"
metadata:
  actual:
    adr_id: 72e79670-f60d-4d60-82f6-98c6d8f9da11
    status: accepted
    candidate_group_id: e7fd5b3d-c86c-4a8d-8a14-d0b1f3338248
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails Module for Internal Email Dispatch: Internal Email Route Endpoints Verify Caller

Scope: applies to `apps/web/src/app/api/internal/email/**`.

These rules are ALWAYS ACTIVE for internal API route handlers responsible for transactional email creation and dispatch, and backend server endpoints triggering team invitations, customer feedback notifications, and system emails.

### Rules

- **R-EMAIL-001** SHOULD: Internal email route endpoints SHOULD verify caller authorization headers against runtime secret configuration prior to invoking @openstatus/emails procedures.
- **R-EMAIL-002** MANDATORY: Discover the dependency manifest in the repo to identify versions, build tools, and lock/resolution artifacts before writing code using versioned libraries.
- **R-EMAIL-003** MANDATORY: Import email templates and delivery utilities directly from @openstatus/emails at the boundary of internal email dispatch endpoints.
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
Claude Code MUST NOT skip or defer verification. Automated workspace type checking, peer code reviews, and verification suites must validate all internal email dispatch endpoints.
</enforcement>