---
version: 2
name: cross-cutting-database-lookups-dispatch-records-associ-28f2
description: "Adoption of @openstatus/emails Module for Internal Email Dispatch: Database Lookups Dispatch Records Associated Email"
metadata:
  actual:
    adr_id: 28f2ab28-9f81-46a6-a0f8-3d6ec2d035f8
    status: accepted
    candidate_group_id: e7fd5b3d-c86c-4a8d-8a14-d0b1f3338248
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails Module for Internal Email Dispatch: Database Lookups Dispatch Records Associated Email

Scope: applies to `apps/web/src/app/api/internal/email/**`.

These rules are ALWAYS ACTIVE for internal API route handlers responsible for transactional email creation and dispatch, and backend server endpoints triggering team invitations, customer feedback notifications, and system emails.

### Rules

- **R-OPEN-001** SHOULD: Database lookups and dispatch records associated with email operations SHOULD utilize shared database schema and client definitions.

### Verify

```bash
# Discover and execute the project verification task for the email module and consuming web route handlers
# Discover and execute the workspace type check and test suites covering internal email API routes
```

**Accept when:**
- All internal email route handlers import email generation and dispatch routines exclusively from the @openstatus/emails library.
- Workspace type checking and project verification test suites pass without errors across all internal email dispatch endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>