---
version: 2
name: cross-cutting-engineers-discover-ecosystem-lock-file-45a1
description: "Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Engineers Discover Ecosystem Lock File Resolve"
metadata:
  actual:
    adr_id: 45a16711-d693-491e-9745-ca66eb7f7ff3
    status: accepted
    candidate_group_id: 009e557d-1a63-4729-89c8-7ee78b28aa15
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Engineers Discover Ecosystem Lock File Resolve

Scope: applies to `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for service boundary message definitions, inter-service contract structures, and distributed telemetry, monitoring definitions, and contract serialization layers.

### Rules

- **R-PROTO-001** MUST: Engineers MUST discover the ecosystem lock file and resolve the exact locked version of the library before implementing or updating boundary contract structures.
- **R-PROTO-002** MUST: Locate the repository dependency manifest to verify the resolved library version and associated reflection runtime packages.
- **R-PROTO-003** MUST: Ensure all contract definitions implement ProtoReflect and Descriptor methods in accordance with the resolved runtime library documentation.
- **R-PROTO-004** MUST: Manage synchronization primitives carefully when registering or accessing raw descriptor data to ensure thread safety across concurrent requests.

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