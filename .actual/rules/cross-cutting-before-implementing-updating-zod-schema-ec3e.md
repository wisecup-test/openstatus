---
version: 2
name: cross-cutting-before-implementing-updating-zod-schema-ec3e
description: "Zod Runtime Schema Validation for Notification Payloads: Before Implementing Updating Zod Schema Definitions"
metadata:
  actual:
    adr_id: ec3ea61e-14e1-4587-9e39-d49bd5a55e3a
    status: accepted
    candidate_group_id: 43f4a8eb-d729-4c10-9edf-7ad646f9d166
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Notification Payloads: Before Implementing Updating Zod Schema Definitions

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for ingress handling, processing of serialized notification payload data across notification provider integrations, and runtime configuration validation for external notification service dispatchers.

### Rules

- **R-ZOD-001** MUST: Before implementing or updating zod schema definitions, consumers MUST inspect the repository dependency lock artifact to determine the exact resolved version of zod and verify API compatibility against version-specific documentation.

### Verify

```bash
# Discover and execute the project test runner script validating payload schema enforcement
# Discover and execute the repository linter script across notification integration modules
```

**Accept when:**
- All notification payload validation tests pass successfully with valid and invalid input fixtures.
- Static analysis and type checking pass without violations across notification modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>