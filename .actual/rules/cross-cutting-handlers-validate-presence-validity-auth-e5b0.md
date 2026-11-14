---
version: 2
name: cross-cutting-handlers-validate-presence-validity-auth-e5b0
description: "Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Handlers Validate Presence Validity Authorization Tokens"
metadata:
  actual:
    adr_id: e5b0dc50-f7cf-4bf2-86c0-21b38b1a0b32
    status: accepted
    candidate_group_id: 35c33ebb-f78e-4e67-899c-0be69ed767c1
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
      - proto/private_location/v1/**
---

# Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Handlers Validate Presence Validity Authorization Tokens

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all external-facing remote procedure call interfaces connecting distributed agents to central ingestion endpoints and server request handlers managing monitor synchronization and protocol telemetry ingestion.

### Rules

- **R-CONN-001** MUST: Handlers MUST validate the presence and validity of authorization tokens from protocol request headers before processing domain operations.

### Verify

```bash
# Discover and execute the repository dependency lock verification script to validate locked protocol library versions.
# Discover and execute the repository test runner targeting all external protocol handler suites.
# Discover and execute the repository static analysis and linting suite to verify protocol contract compliance.
```

**Accept when:**
- All external RPC endpoints register handlers adhering to the generated service interface specifications.
- RPC handlers accept typed request containers, return typed response containers, and report errors using protocol status codes.
- Static contract and lock file verification checks pass without unauthenticated or untyped handler regressions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon detecting untyped external endpoints or bypassed protocol request envelopes. Pull requests introducing non-compliant error codes or missing authentication checks are blocked from merging.
</enforcement>