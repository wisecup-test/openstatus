---
version: 2
name: cross-cutting-internal-route-handlers-not-implement-b1f9
description: "Adoption of @openstatus/emails for Centralized Email Notification Handling: Internal Route Handlers Not Implement Custom"
metadata:
  actual:
    adr_id: b1f9d6b2-71a5-4a28-882f-b0ae1f77d602
    status: accepted
    candidate_group_id: 83171de2-78b9-4015-98c5-6b0874312f42
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails for Centralized Email Notification Handling: Internal Route Handlers Not Implement Custom

Scope: applies to `apps/web/src/app/api/internal/email/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EMAIL-001** MUST_NOT: Internal route handlers MUST NOT implement custom HTML or plain-text email generation outside the @openstatus/emails package boundary.

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
Claude Code MUST NOT skip or defer verification. Continuous integration checks fail when unauthorized email formatting or untracked email imports are detected in route handlers.
</enforcement>