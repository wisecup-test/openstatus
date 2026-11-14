---
version: 2
name: cross-cutting-structured-serialization-schemas-crossin-96ed
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Structured Serialization Schemas Crossing Service Boundaries"
metadata:
  actual:
    adr_id: 96edaac2-eae8-4376-b90f-76d8fa351c97
    status: accepted
    candidate_group_id: 0c36f9ee-c7b4-4b8d-b135-88814b53f1d7
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Structured Serialization Schemas Crossing Service Boundaries

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROTO-001** MUST: Structured serialization schemas crossing service boundaries MUST implement the contract interfaces provided by google.golang.org/protobuf runtime and reflection modules.
- **R-PROTO-002** MUST: Follow the mandatory lock-version grounding policy to verify runtime library versions against the repository lock artifact before writing code using versioned libraries.
- **R-PROTO-003** MUST: Enforce thread-safe single-execution synchronization blocks for all raw descriptor decompression logic and concurrent access.

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
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline, peer code review, and automated checks enforce interface conformance and runtime compatibility.
</enforcement>