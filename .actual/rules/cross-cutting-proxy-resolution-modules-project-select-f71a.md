---
version: 2
name: cross-cutting-proxy-resolution-modules-project-select-f71a
description: "Adoption of @openstatus/db/src/schema for Proxy Resolution and Access Control: Proxy Resolution Modules Project Select Narrow"
metadata:
  actual:
    adr_id: f71a5dc7-4ef0-4b9a-9167-83b1122922f2
    status: accepted
    candidate_group_id: f8b4ae84-5a62-426c-ba59-a50291856ca2
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/proxy/**
---

# Adoption of @openstatus/db/src/schema for Proxy Resolution and Access Control: Proxy Resolution Modules Project Select Narrow

Scope: applies to `apps/status-page/src/lib/proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROXY-001** MAY: Proxy resolution modules MAY project or select narrow subsets of schema types to avoid retaining unnecessary data structures during request dispatch.

### Verify

```bash
# Discover and run the repository type-checking script to verify cross-package schema compatibility across proxy resolution handlers.
# Discover and execute the test runner for proxy routing resolution to validate access predicate behavior against schema models.
# Discover and execute the repository dependency boundary linter to confirm adherence to package import boundaries.
```

**Accept when:**
- Type checking passes without errors across all proxy resolution handlers importing @openstatus/db/src/schema.
- All proxy access control and rewrite unit tests pass successfully.
- No duplicate local interface definitions exist for entities provided by @openstatus/db/src/schema.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>