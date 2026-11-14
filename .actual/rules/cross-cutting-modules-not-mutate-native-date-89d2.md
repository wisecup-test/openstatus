---
version: 2
name: cross-cutting-modules-not-mutate-native-date-89d2
description: "Adoption of date-fns for Functional Date and Time Manipulation: Modules Not Mutate Native Date Instances"
metadata:
  actual:
    adr_id: 89d2e7df-0b6e-41cc-a6c9-b6b9f82429bb
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

# Adoption of date-fns for Functional Date and Time Manipulation: Modules Not Mutate Native Date Instances

Scope: applies to `apps/web/src/app/api/og/status/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application utility modules, domain calculation helpers, and API route handlers performing date arithmetic, interval computation, or timestamp formatting.

### Rules

- **R-DATE-001** MUST_NOT: Modules MUST_NOT mutate native date instances in place, and MUST rely on pure functional transforms that return new date representations.

### Verify

```bash
# Discover the project dependency manifest and lock artifact, identify the project verification script, and execute it to confirm dependency resolution and module integrity.
# Inspect repository configuration to locate the project linting and static analysis commands, and execute them to ensure all date transformations comply with import standards.
```

**Accept when:**
- All modules performing date or time manipulation import and execute date-fns functions without errors during project verification.
- Static type checking and automated test suites discover no native Date mutations or unapproved alternative date libraries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All code changes involving temporal calculations and utility formatting are subject to automated linting, static type analysis, and peer review.
</enforcement>