---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-34d5
description: "Adoption of date-fns for Functional Date and Time Manipulation: Consumers Inspect Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: 34d5f4c9-f8be-46b9-aab2-e49ef743a552
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

# Adoption of date-fns for Functional Date and Time Manipulation: Consumers Inspect Repository Dependency Manifest Authoritative

Scope: applies to `apps/web/src/app/api/og/status/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application utility modules, domain calculation helpers, and API route handlers performing date arithmetic, interval computation, or timestamp formatting.

### Rules

- **R-DATE-001** MUST: Consumers MUST inspect the repository dependency manifest and authoritative resolution lock artifact to determine the exact resolved version of date-fns and verify API availability against version documentation before implementation.

### Verify

```bash
# Discover the project dependency manifest and lock artifact, identify the project verification script, and execute it to confirm dependency resolution and module integrity.
# Inspect repository configuration to locate the project linting and static analysis commands, and execute them to ensure all date transformations comply with import standards.
```

**Accept when:**
- All modules performing date or time manipulation import and execute date-fns functions without errors during project verification.
- Static type checking and automated test suites discover no native Date mutations or unapproved alternative date libraries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes involving temporal calculations and utility formatting are verified by automated linting rules, static type analysis, and peer review.
</enforcement>