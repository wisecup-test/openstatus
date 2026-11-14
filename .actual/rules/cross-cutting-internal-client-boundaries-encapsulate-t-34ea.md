---
version: 2
name: cross-cutting-internal-client-boundaries-encapsulate-t-34ea
description: "Internal API Contract Standardization with google.golang.org/protobuf: Internal Client Boundaries Encapsulate Transport Serialization"
metadata:
  actual:
    adr_id: 34eafa82-df86-4b23-a711-8fd299641f08
    status: accepted
    candidate_group_id: 523c2937-cde9-4774-8810-baf86298bb1a
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
      - internal/tinybird/**
      - internal/workflows/**
      - proto/private_location/v1/**
---

# Internal API Contract Standardization with google.golang.org/protobuf: Internal Client Boundaries Encapsulate Transport Serialization

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-INT-001** SHOULD: Internal client boundaries SHOULD encapsulate transport and serialization concerns behind dedicated client interfaces.

### Verify

```bash
# Discover and execute project contract linting and verification scripts
# Discover and execute project test suite covering internal API serialization and message boundary validation
```

**Accept when:**
- All internal service communication schemas implement the ProtoMessage and ProtoReflect interfaces without manual field mutation.
- Automated contract verification confirms zero schema drift and successful binary serialization across all internal API endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis checks, schema linters, and automated boundary test suites verify compliance. Violations are blocked by CI gates or require refactoring.
</enforcement>