---
version: 2
name: cross-cutting-internal-notification-route-handlers-cen-a33e
description: "Adoption of @openstatus/emails for Centralized Email Notification Handling: Internal Notification Route Handlers Centralize Email"
metadata:
  actual:
    adr_id: a33e1aeb-b4d8-4acc-a5db-906de5f8aa59
    status: accepted
    candidate_group_id: 83171de2-78b9-4015-98c5-6b0874312f42
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails for Centralized Email Notification Handling: Internal Notification Route Handlers Centralize Email

Scope: applies to `apps/web/src/app/api/internal/email/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** MUST: Internal notification route handlers MUST centralize email composition and rendering through the @openstatus/emails internal library module rather than defining inline email formatting.

### Verify

```bash
# Discover and execute the project verification script declared in the repository manifest to validate module dependency boundaries and type contracts.
# Run the automated test runner discovered from the build configuration to verify internal notification route execution against email contracts.
```

**Accept when:**
- All internal route handlers generating notifications compile cleanly against the shared email module without type or module resolution errors.
- Automated test suites for internal endpoints execute successfully and verify email generation contracts.
- Static dependency analysis confirms that notification handlers import email templates exclusively through the designated internal package.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>