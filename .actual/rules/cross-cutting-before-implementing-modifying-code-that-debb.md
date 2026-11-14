---
version: 2
name: cross-cutting-before-implementing-modifying-code-that-debb
description: "Adoption of @openstatus/emails for Centralized Email Notification Handling: Before Implementing Modifying Code That Depends"
metadata:
  actual:
    adr_id: debbbda2-2d88-4924-9343-df40bd683249
    status: accepted
    candidate_group_id: 83171de2-78b9-4015-98c5-6b0874312f42
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails for Centralized Email Notification Handling: Before Implementing Modifying Code That Depends

Scope: applies to `apps/web/src/app/api/internal/email/**`.

These rules are ALWAYS ACTIVE for all internal server route handlers and endpoints responsible for creating, formatting, or dispatching transactional emails.

### Rules

- **R-OPENSTATUS-001** MUST: Before implementing or modifying code that depends on @openstatus/emails, developers MUST inspect the repository lock artifact to determine the exact resolved dependency version and verify API compatibility against that version.

### Verify

```bash
# Discover and execute project verification and test scripts declared in the repository manifest
# Example discovery loop:
# 1. Identify the manifest (package.json, Cargo.toml, go.mod, etc.)
# 2. Extract and run verification/test scripts
```

**Accept when:**
- All internal route handlers generating notifications compile cleanly against the shared email module without type or module resolution errors.
- Automated test suites for internal endpoints execute successfully and verify email generation contracts.
- Static dependency analysis confirms that notification handlers import email templates exclusively through the designated internal package.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Continuous integration checks fail when unauthorized email formatting or untracked email imports are detected in route handlers, and pull requests violating email package encapsulation are blocked until refactored.
</enforcement>