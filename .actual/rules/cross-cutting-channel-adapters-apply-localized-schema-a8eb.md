---
version: 2
name: cross-cutting-channel-adapters-apply-localized-schema-a8eb
description: "Zod Schema Validation for Subscription Channel Configurations and Payloads: Channel Adapters Apply Localized Schema Extensions"
metadata:
  actual:
    adr_id: a8eb31b7-05b4-42df-81c1-2b33e78f0bdc
    status: accepted
    candidate_group_id: 500f8313-2cab-4db3-a165-4a1aab0ad2dd
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Zod Schema Validation for Subscription Channel Configurations and Payloads: Channel Adapters Apply Localized Schema Extensions

Scope: applies to `packages/subscriptions/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MAY: Channel adapters MAY apply localized schema extensions to accommodate transport-specific metadata when the base configuration passes validation.
- **R-ZOD-002** MANDATORY: Discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ZOD-003** MANDATORY: Before writing code that uses a versioned library, find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-verify per dependency at point of use.
- **R-ZOD-004** MANDATORY: Define channel configuration schemas alongside their respective channel handlers to maintain high cohesion.
- **R-ZOD-005** MANDATORY: Use non-throwing safe parsing methods when evaluating configuration data to handle malformed input gracefully.
- **R-ZOD-006** MANDATORY: Compose complex event payload schemas from reusable domain primitive schemas rather than defining inline duplicates.

### Verify

```bash
test -n "${VERIFY_SCHEMA_VALIDATION:-}" && eval "${VERIFY_SCHEMA_VALIDATION}" || echo "Discover and execute project test suite for schema validation compliance"
test -n "${VERIFY_CODE_STANDARDS:-}" && eval "${VERIFY_CODE_STANDARDS}" || echo "Discover and execute project static analysis and linting checks"
```

**Accept when:**
- All channel configuration parsing and payload formatting paths pass automated boundary validation tests with valid schemas.
- Invalid configuration strings or corrupted event payloads fail safely without uncaught runtime exceptions during dispatch.
- The locked dependency version for the schema validation library is verified against repository lock artifacts prior to execution.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>