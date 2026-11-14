---
version: 2
name: cross-cutting-boundary-contract-data-structures-adopt-d776
description: "Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Boundary Contract Data Structures Adopt Google"
metadata:
  actual:
    adr_id: d7767362-2d5d-4397-aaf9-b889c04384df
    status: accepted
    candidate_group_id: 009e557d-1a63-4729-89c8-7ee78b28aa15
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Boundary Contract Data Structures Adopt Google

Scope: applies to `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for service boundary message definitions, inter-service contract structures, distributed telemetry, monitoring definitions, and contract serialization layers.

### Rules

- **R-ADR-001** MUST: All boundary contract data structures MUST adopt google.golang.org/protobuf runtime and reflection modules to define message schemas, implement ProtoMessage, and provide ProtoReflect contract interfaces.

### Verify

```bash
# Discover and execute the repository contract validation scripts to verify schema integrity and generated model alignment.
# Execute the repository test suite to validate serialization, deserialization, and reflection behavior across all boundary contracts.
```

**Accept when:**
- All contract validation scripts execute without reporting schema discrepancies or outdated contract structures.
- All boundary serialization and reflection test suites pass with zero errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>