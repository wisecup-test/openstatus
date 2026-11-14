---
version: 2
name: cross-cutting-consumers-discover-ecosystem-lock-file-b8e5
description: "@chronark/zod-bird Client Token Configuration: Consumers Discover Ecosystem Lock File Verify"
metadata:
  actual:
    adr_id: b8e51cf6-c5bf-4434-941c-745475516594
    status: accepted
    candidate_group_id: aaeaef12-e722-4be9-af54-ce9501b3057a
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Token Configuration: Consumers Discover Ecosystem Lock File Verify

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Consumers MUST discover the ecosystem lock file and verify the exact resolved version of @chronark/zod-bird before calling client initialization or query APIs.
- **R-ZOD-002** MUST: Inject client tokens through application configuration factories or environment readers rather than referencing raw global state directly within business logic.
- **R-ZOD-003** MUST: Ensure warning handlers or fallback pipelines alert operators when token configuration is absent so degraded analytics responses can be differentiated from empty query results.

### Verify

```bash
# Discover and run the project's static analysis and secret detection checks to ensure no hardcoded API keys exist in client integration code.
# Discover and execute the test suite for client initialization to verify that missing credentials trigger appropriate diagnostic warnings and fallback behavior.
```

**Accept when:**
- Static secret scanning passes with zero hardcoded authentication tokens detected in client source files.
- Client initialization tests pass, confirming successful token propagation to Client instances and appropriate warning emissions when credentials are unconfigured.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration static secret scanning, peer code review, and automated test execution.
</enforcement>