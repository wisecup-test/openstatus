---
version: 2
name: cross-cutting-internal-service-clients-handlers-consum-a5eb
description: "Internal API Contract Standardization with google.golang.org/protobuf: Internal Service Clients Handlers Consume Compiled"
metadata:
  actual:
    adr_id: a5eb907c-a706-4afe-a313-c8845615bfd6
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

# Internal API Contract Standardization with google.golang.org/protobuf: Internal Service Clients Handlers Consume Compiled

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROT-001** SHOULD: Internal service clients and handlers SHOULD consume compiled message descriptor data through thread-safe, one-time initialization rather than per-request descriptor parsing.

### Verify

```bash
# Discover and execute project contract linting and verification scripts
# Discover and execute project test suite covering internal API serialization and message boundary validation
```

**Accept when:**
- All internal service communication schemas implement the ProtoMessage and ProtoReflect interfaces without manual field mutation.
- Automated contract verification confirms zero schema drift and successful binary serialization across all internal API endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. All internal service communication schemas must conform to the compiled protobuf message contracts and verification must pass.
</enforcement>