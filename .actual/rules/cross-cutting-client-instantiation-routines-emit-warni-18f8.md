---
version: 2
name: cross-cutting-client-instantiation-routines-emit-warni-18f8
description: "@chronark/zod-bird Client Token Configuration: Client Instantiation Routines Emit Warning Diagnostics"
metadata:
  actual:
    adr_id: 18f8a487-fd78-4793-8818-bd704c79955d
    status: accepted
    candidate_group_id: aaeaef12-e722-4be9-af54-ce9501b3057a
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Token Configuration: Client Instantiation Routines Emit Warning Diagnostics

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** SHOULD: Client instantiation routines SHOULD emit warning diagnostics and return empty data structures when secret tokens are omitted in non-production environments.
- **R-ZOD-002** MANDATORY: Inject client tokens through application configuration factories or environment readers rather than referencing raw global state directly within business logic.
- **R-ZOD-003** MANDATORY: Ensure warning handlers or fallback pipelines alert operators when token configuration is absent so degraded analytics responses can be differentiated from empty query results.

### Verify

```bash
# Discover and run the project's static analysis and secret detection checks to ensure no hardcoded API keys exist in client integration code.
# Discover and execute the test suite for client initialization to verify that missing credentials trigger appropriate diagnostic warnings and fallback behavior.
```

**Accept when:**
- Static secret scanning passes with zero hardcoded authentication tokens detected in client source files.
- Client initialization tests pass, confirming successful token propagation to Client instances and appropriate warning emissions when credentials are unconfigured.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static secret scanning, peer code reviews, and automated test execution verify compliance.
</enforcement>