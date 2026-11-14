---
version: 2
name: cross-cutting-message-schemas-implement-protomessage-p-d5ba
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Message Schemas Implement Protomessage Protoreflect Interface"
metadata:
  actual:
    adr_id: d5bacbd3-aa8e-4a37-8938-586b4a110f72
    status: accepted
    candidate_group_id: 0c36f9ee-c7b4-4b8d-b135-88814b53f1d7
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Message Schemas Implement Protomessage Protoreflect Interface

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all data structures and schemas defining network protocols, monitor configurations, assertion schemas, and telemetry records crossing module or service boundaries.

### Rules

- **R-PROTO-001** MUST: Message schemas MUST implement the ProtoMessage and ProtoReflect interface contracts to expose standard descriptor metadata and reflection capabilities.

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