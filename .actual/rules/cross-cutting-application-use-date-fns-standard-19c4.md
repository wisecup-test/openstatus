---
version: 2
name: cross-cutting-application-use-date-fns-standard-19c4
description: "Adoption of date-fns for Functional Date and Time Manipulation: Application Use Date Fns Standard Library"
metadata:
  actual:
    adr_id: 19c4c70d-909b-493d-b63a-1122c454fcf0
    status: accepted
    candidate_group_id: e914c52b-35bd-4db1-bc0f-89fb166ab748
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/status/**
      - src/lib/**
---

# Adoption of date-fns for Functional Date and Time Manipulation: Application Use Date Fns Standard Library

Scope: applies to `apps/web/src/app/api/og/status/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application utility modules, domain calculation helpers, and API route handlers performing date arithmetic, interval computation, or timestamp formatting.

### Rules

- **R-DATE-001** MUST: The application MUST use date-fns as the standard library for all date parsing, arithmetic, interval comparison, and formatting operations across shared utilities and API routes.

### Verify

```bash
# Discover the project dependency manifest and lock artifact, identify the project verification script, and execute it to confirm dependency resolution and module integrity.
# Inspect repository configuration to locate the project linting and static analysis commands, and execute them to ensure all date transformations comply with import standards.
```

**Accept when:**
- All modules performing date or time manipulation import and execute date-fns functions without errors during project verification.
- Static type checking and automated test suites discover no native Date mutations or unapproved alternative date libraries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>