---
version: 2
name: cross-cutting-procedure-inputs-passed-openstatus-servi-df53
description: "Primary Datastore Access Delegation via @openstatus/services: Procedure Inputs Passed Openstatus Services Functions"
metadata:
  actual:
    adr_id: df536444-c075-47fd-93c8-fc2e95871f96
    status: accepted
    candidate_group_id: ba6c3eef-91c9-4cd0-a5dc-ed506fd87311
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Primary Datastore Access Delegation via @openstatus/services: Procedure Inputs Passed Openstatus Services Functions

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ODP-001** MUST: All procedure inputs passed into @openstatus/services functions MUST be validated against schema validators before passing the sanitized parameters to the service layer.

### Verify

```bash
# Discover and run the project build and type-checking scripts
# Discover and run the test suite scripts declared in the workspace configuration
```

**Accept when:**
- All API procedures interacting with primary datastores delegate persistence operations through @openstatus/services domain modules without direct datastore client invocation.
- Static analysis and type checking confirm zero type errors across context adapters, input validators, and service invocations.
- Automated test suites pass with verification of service error handling and tenant boundary isolation.

<enforcement>
Claude Code MUST NOT skip or defer verification. All API procedures interacting with primary datastores MUST delegate persistence operations through @openstatus/services domain modules.
</enforcement>