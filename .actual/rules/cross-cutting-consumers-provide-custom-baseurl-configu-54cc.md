---
version: 2
name: cross-cutting-consumers-provide-custom-baseurl-configu-54cc
description: "@chronark/zod-bird Client Token Configuration: Consumers Provide Custom Baseurl Configuration Property"
metadata:
  actual:
    adr_id: 54cc865e-d2db-45ae-9949-0ff09c3f8568
    status: accepted
    candidate_group_id: aaeaef12-e722-4be9-af54-ce9501b3057a
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Token Configuration: Consumers Provide Custom Baseurl Configuration Property

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MAY: Consumers MAY provide a custom baseUrl configuration property alongside the authentication token when instantiating the @chronark/zod-bird Client.

### Verify

```bash
# Discover and run the project's static analysis and secret detection checks to ensure no hardcoded API keys exist in client integration code.
# Discover and execute the test suite for client initialization to verify that missing credentials trigger appropriate diagnostic warnings and fallback behavior.
```

**Accept when:**
- Static secret scanning passes with zero hardcoded authentication tokens detected in client source files.
- Client initialization tests pass, confirming successful token propagation to Client instances and appropriate warning emissions when credentials are unconfigured.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>