---
version: 2
name: cross-cutting-deserialization-routines-validate-field-5601
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Deserialization Routines Validate Field Descriptors Structural"
metadata:
  actual:
    adr_id: 5601c531-5f23-43ec-b1bf-0ac00b285c0b
    status: accepted
    candidate_group_id: 0c36f9ee-c7b4-4b8d-b135-88814b53f1d7
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Deserialization Routines Validate Field Descriptors Structural

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROTO-001** SHOULD: Deserialization routines SHOULD validate field descriptors and structural invariants via Descriptor metadata before processing external payload buffers.

### Verify

```bash
# Find the repository build and validation script defined in the project configuration and execute it to verify message schema compilation.
# Locate the contract test execution command from the project manifest and run test suites validating ProtoMessage serialization and reflection behaviors.
# Query the repository dependency auditing mechanism to confirm all references to the serialization runtime resolve to the locked dependency version.
```

**Accept when:**
- All message definitions pass validation and schema generation checks without compilation errors.
- Test suites asserting message serialization, deserialization, and contract interface compliance pass completely.
- The resolved library version matches the exact specification recorded in the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>