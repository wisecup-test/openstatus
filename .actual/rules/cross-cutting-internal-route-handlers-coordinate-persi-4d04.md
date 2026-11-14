---
version: 2
name: cross-cutting-internal-route-handlers-coordinate-persi-4d04
description: "Adoption of @openstatus/emails for Centralized Email Notification Handling: Internal Route Handlers Coordinate Persistence Authorization"
metadata:
  actual:
    adr_id: 4d04c159-604b-40c0-95d6-d9aa26e89274
    status: accepted
    candidate_group_id: 83171de2-78b9-4015-98c5-6b0874312f42
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails for Centralized Email Notification Handling: Internal Route Handlers Coordinate Persistence Authorization

Scope: applies to `apps/web/src/app/api/internal/email/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EMAIL-001** SHOULD: Internal route handlers SHOULD coordinate persistence and authorization workflows independently while delegating all message presentation structuring to @openstatus/emails.

### Verify

```bash
# Discover and execute project verification and test scripts from the repository manifest
```

**Accept when:**
- All internal route handlers generating notifications compile cleanly against the shared email module without type or module resolution errors.
- Automated test suites for internal endpoints execute successfully and verify email generation contracts.
- Static dependency analysis confirms that notification handlers import email templates exclusively through the designated internal package.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated module import lint rules, dependency boundary analysis, and mandatory peer review enforce these rules.
</enforcement>