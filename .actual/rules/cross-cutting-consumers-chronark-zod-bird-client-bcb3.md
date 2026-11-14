---
version: 2
name: cross-cutting-consumers-chronark-zod-bird-client-bcb3
description: "@chronark/zod-bird Client Token Configuration: Consumers Chronark Zod Bird Client Library"
metadata:
  actual:
    adr_id: bcb39615-a9b9-4f7d-a7cd-6ffc7a21f134
    status: accepted
    candidate_group_id: aaeaef12-e722-4be9-af54-ce9501b3057a
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Token Configuration: Consumers Chronark Zod Bird Client Library

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for client initialization and credential provisioning for analytics pipes, and modules managing external API secrets and authentication tokens.

### Rules

- **R-ZOD-001** MUST: Consumers of the @chronark/zod-bird client library MUST instantiate Client instances by supplying API tokens through explicit configuration objects derived from runtime environment variables rather than embedding credentials in application code.

### Verify

```bash
# Discover and run the project's static analysis and secret detection checks
# Discover and execute the test suite for client initialization
```

**Accept when:**
- Static secret scanning passes with zero hardcoded authentication tokens detected in client source files.
- Client initialization tests pass, confirming successful token propagation to Client instances and appropriate warning emissions when credentials are unconfigured.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static secret scanning, peer code review, and automated test execution will block violations.
</enforcement>