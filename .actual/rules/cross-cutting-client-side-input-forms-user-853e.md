---
version: 2
name: cross-cutting-client-side-input-forms-user-853e
description: "Zod Schema Validation for Form and Component Input Boundaries: Client Side Input Forms User Submitted"
metadata:
  actual:
    adr_id: 853e776d-9018-4f24-af63-55e084709368
    status: accepted
    candidate_group_id: df6eea8a-3ab7-4843-bbef-10688bb78119
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/components/**
      - src/lib/**
      - src/scripts/**
---

# Zod Schema Validation for Form and Component Input Boundaries: Client Side Input Forms User Submitted

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/**`, `apps/dashboard/src/scripts/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Client-side input forms and user-submitted data boundaries MUST validate all input payloads against declarative Zod schemas before triggering downstream state updates or network submissions.

### Verify

```bash
# Discover and execute project static analysis scripts to verify Zod schema binding
# Discover and execute test runner suite to validate input schemas
```

**Accept when:**
- All user-facing form components bind an explicit Zod validation schema through the resolver binding without unvalidated input paths.
- Static type checking and automated test runner suites pass with zero validation or schema mismatch errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All user-facing forms and user-submitted data boundaries must be validated against declarative Zod schemas.
</enforcement>