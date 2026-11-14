---
version: 2
name: cross-cutting-message-data-structures-implement-protom-2fcc
description: "Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Message Data Structures Implement Protomessage Protoreflect"
metadata:
  actual:
    adr_id: 2fccfa6c-b90a-4be3-a521-7ae4ff2885f9
    status: accepted
    candidate_group_id: f13253cc-8873-43d0-88ed-e6192e7e6c56
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Message Data Structures Implement Protomessage Protoreflect

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROTO-001** MUST: Message data structures MUST implement the ProtoMessage and ProtoReflect interfaces to guarantee contract introspection and interoperable wire serialization.

### Verify

```bash
# Discover and execute the repository contract validation test suite
# Execute workspace static analysis checks to verify descriptor initialization
```

**Accept when:**
- All boundary message contracts satisfy ProtoMessage, ProtoReflect, and Descriptor interface contracts without runtime reflection faults.
- Test suites across all schema-dependent components pass with zero concurrent descriptor initialization panics.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>