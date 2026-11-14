---
version: 2
name: cross-cutting-descriptor-initialization-executed-via-s-2b4f
description: "Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Descriptor Initialization Executed Via Synchronization Primitives"
metadata:
  actual:
    adr_id: 2b4fc16b-d6dd-491d-b03b-10517280b3a9
    status: accepted
    candidate_group_id: f13253cc-8873-43d0-88ed-e6192e7e6c56
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Descriptor Initialization Executed Via Synchronization Primitives

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all structured domain models, client communication contracts, and monitoring specifications requiring serialization or schema reflection.

### Rules

- **R-PROTO-001** MUST: Descriptor initialization MUST be executed via synchronization primitives and protoimpl compression helpers to guarantee thread-safe, single-pass descriptor registration.

### Verify

```bash
# Discover and execute the repository contract validation test suite to confirm boundary structures satisfy ProtoMessage and ProtoReflect interfaces
go test -v ./...

# Execute workspace static analysis checks to verify descriptor initialization conforms to synchronized single-execution patterns
go vet ./...
```

**Accept when:**
- All boundary message contracts satisfy ProtoMessage, ProtoReflect, and Descriptor interface contracts without runtime reflection faults.
- Test suites across all schema-dependent components pass with zero concurrent descriptor initialization panics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated repository verification checks enforce interface compliance for all boundary payload structures.
</enforcement>