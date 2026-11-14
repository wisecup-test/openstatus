---
version: 2
name: cross-cutting-consumer-discover-ecosystem-lock-file-086b
description: "Internal API Contract Standardization with google.golang.org/protobuf: Consumer Discover Ecosystem Lock File Resolve"
metadata:
  actual:
    adr_id: 086b1d40-d7c4-4c21-904e-b8105c3dd471
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

# Internal API Contract Standardization with google.golang.org/protobuf: Consumer Discover Ecosystem Lock File Resolve

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all internal service contracts, message schemas, and telemetry models exchanged across private location service boundaries.

### Rules

- **R-PROTO-001** MUST: The consumer MUST discover the ecosystem lock file and resolve the exact locked version of google.golang.org/protobuf before implementing or updating internal API contract structures.

### Verify

```bash
# Discover and execute the project contract linting and verification scripts
# Discover and execute the project test suite covering internal API serialization and message boundary validation
```

**Accept when:**
- All internal service communication schemas implement the ProtoMessage and ProtoReflect interfaces without manual field mutation.
- Automated contract verification confirms zero schema drift and successful binary serialization across all internal API endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. All internal API contract modifications must be verified via static analysis, schema linters, and automated boundary test suites.
</enforcement>