---
version: 2
name: cross-cutting-internal-email-rendering-layout-composit-525b
description: "Adoption of @openstatus/emails Module for Internal Email Dispatch: Internal Email Rendering Layout Composition Dispatch"
metadata:
  actual:
    adr_id: 525b1e10-3db5-4bf7-94af-46305cce18b6
    status: accepted
    candidate_group_id: e7fd5b3d-c86c-4a8d-8a14-d0b1f3338248
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails Module for Internal Email Dispatch: Internal Email Rendering Layout Composition Dispatch

Scope: applies to `apps/web/src/app/api/internal/email/**`.

These rules are ALWAYS ACTIVE for internal API route handlers, backend server endpoints triggering team invitations, customer feedback notifications, and system emails.

### Rules

- **R-EMAIL-001** MUST: All internal email rendering, layout composition, and dispatch execution within server endpoints MUST use the @openstatus/emails library rather than inline markup or external notification clients.
- **R-EMAIL-002** MUST: Import email templates and delivery utilities directly from @openstatus/emails at the boundary of internal email dispatch endpoints.
- **R-EMAIL-003** MUST: Ensure authorization verification executes prior to invoking email template rendering or delivery procedures.
- **R-EMAIL-004** MUST: Execute LOCK-VERSION GROUNDING before writing code that uses a versioned library: find dependency manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm APIs exist, and re-verify per dependency at point of use.

### Verify

```bash
# Discover and execute the project verification task for the email module and consuming web route handlers
# Discover and execute workspace type check and test suites covering internal email API routes
```

**Accept when:**
- All internal email route handlers import email generation and dispatch routines exclusively from the @openstatus/emails library.
- Workspace type checking and project verification test suites pass without errors across all internal email dispatch endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated workspace type checking, verification suites, and peer code reviews enforce compliance.
</enforcement>