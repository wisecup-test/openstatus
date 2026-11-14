---
version: 2
name: cross-cutting-domain-components-not-bypass-protoreflec-2440
description: "Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Domain Components Not Bypass Protoreflect Descriptor"
metadata:
  actual:
    adr_id: 24407c99-bb97-4110-995f-8b648b6f6ee9
    status: accepted
    candidate_group_id: f13253cc-8873-43d0-88ed-e6192e7e6c56
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Domain Components Not Bypass Protoreflect Descriptor

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all structured domain models, client communication contracts, and monitoring specifications requiring serialization or schema reflection.

### Rules

- **R-PROTO-001** MUST_NOT: Domain components MUST NOT bypass the protoreflect descriptor abstraction to manipulate internal serialized representations through raw memory offsets or direct descriptor mutation.

### Verify

```bash
# Discover and execute the repository contract validation test suite to confirm all boundary structures satisfy ProtoMessage and ProtoReflect interfaces.
# Execute the workspace static analysis checks to verify that descriptor initialization conforms to synchronized single-execution patterns without unauthorized memory manipulation.
```

**Accept when:**
- All boundary message contracts satisfy ProtoMessage, ProtoReflect, and Descriptor interface contracts without runtime reflection faults.
- Test suites across all schema-dependent components pass with zero concurrent descriptor initialization panics.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>