---
version: 2
name: cross-cutting-route-handlers-initiating-transactional-dc60
description: "Adoption of @openstatus/emails for Centralized Email Notification Handling: Route Handlers Initiating Transactional Notifications Import"
metadata:
  actual:
    adr_id: dc60f613-929d-4588-9e00-6c0162fb7f75
    status: accepted
    candidate_group_id: 83171de2-78b9-4015-98c5-6b0874312f42
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails for Centralized Email Notification Handling: Route Handlers Initiating Transactional Notifications Import

Scope: applies to `apps/web/src/app/api/internal/email/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-EMAILS-001** MUST: Route handlers initiating transactional notifications MUST import email templates and rendering functions directly from @openstatus/emails.

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