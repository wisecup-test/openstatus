---
version: 2
name: cross-cutting-internal-contract-message-structures-imp-7176
description: "Internal API Contract Standardization with google.golang.org/protobuf: Internal Contract Message Structures Implement Protomessage"
metadata:
  actual:
    adr_id: 71762f3e-2768-4378-af37-b5a9f0b5e413
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

# Internal API Contract Standardization with google.golang.org/protobuf: Internal Contract Message Structures Implement Protomessage

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all internal service contracts, message schemas, and telemetry models exchanged across private location service boundaries, and all internal monitor configuration specifications, execution requests, and assertion payloads.

### Rules

- **R-PROTO-001** MUST: All internal contract message structures MUST implement the ProtoMessage and ProtoReflect interfaces provided by google.golang.org/protobuf.

### Verify

```bash
# Discover and execute the project contract linting and verification scripts
# Discover and execute the project test suite covering internal API serialization and message boundary validation
```

**Accept when:**
- All internal service communication schemas implement the ProtoMessage and ProtoReflect interfaces without manual field mutation.
- Automated contract verification confirms zero schema drift and successful binary serialization across all internal API endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by static analysis checks, schema linters in continuous integration pipelines, automated boundary test suites, and architecture peer reviews.
</enforcement>