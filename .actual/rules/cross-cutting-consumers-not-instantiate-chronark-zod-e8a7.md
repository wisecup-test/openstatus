---
version: 2
name: cross-cutting-consumers-not-instantiate-chronark-zod-e8a7
description: "@chronark/zod-bird Client Token Configuration: Consumers Not Instantiate Chronark Zod Bird"
metadata:
  actual:
    adr_id: e8a7bb75-1da2-40fd-863d-3e82cdd5d1af
    status: accepted
    candidate_group_id: aaeaef12-e722-4be9-af54-ce9501b3057a
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Token Configuration: Consumers Not Instantiate Chronark Zod Bird

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for client initialization, credential provisioning, and modules managing external API secrets and authentication tokens.

### Rules

- **R-ZOD-001** MUST_NOT: Consumers MUST NOT instantiate the @chronark/zod-bird Client with hardcoded secret values or fallback test credentials in source code.

### Verify

```bash
# Discover and run the project's static analysis and secret detection checks
# Discover and execute the test suite for client initialization
```

**Accept when:**
- Static secret scanning passes with zero hardcoded authentication tokens detected in client source files.
- Client initialization tests pass, confirming successful token propagation to Client instances and appropriate warning emissions when credentials are unconfigured.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>