---
version: 2
name: cross-cutting-components-not-mutate-internal-raw-7bd7
description: "Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Components Not Mutate Internal Raw Descriptor"
metadata:
  actual:
    adr_id: 7bd79f6c-022d-49a9-b5e3-6ae8af0232c6
    status: accepted
    candidate_group_id: 009e557d-1a63-4729-89c8-7ee78b28aa15
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Components Not Mutate Internal Raw Descriptor

Scope: applies to `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all service boundary message definitions, inter-service contract structures, and distributed telemetry/monitoring definitions.

### Rules

- **R-PROTO-001** MUST_NOT: Components MUST_NOT mutate internal raw descriptor data structures after initial synchronization and registration.

### Verify

```bash
# Discover and execute the repository contract validation scripts to verify schema integrity and generated model alignment.
# Execute the repository test suite to validate serialization, deserialization, and reflection behavior across all boundary contracts.
```

**Accept when:**
- All contract validation scripts execute without reporting schema discrepancies or outdated contract structures.
- All boundary serialization and reflection test suites pass with zero errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated contract consistency checks and peer code reviews verify adherence to runtime and reflection requirements.
</enforcement>