---
version: 2
name: cross-cutting-boundary-message-implementations-provide-9167
description: "Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Boundary Message Implementations Provide Standard Contract"
metadata:
  actual:
    adr_id: 91676e2a-2267-4a55-b82e-d0e2539dde40
    status: accepted
    candidate_group_id: f13253cc-8873-43d0-88ed-e6192e7e6c56
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Boundary Message Implementations Provide Standard Contract

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all structured domain models, client communication contracts, and monitoring specifications requiring serialization or schema reflection.

### Rules

- **R-PROTO-001** SHOULD: Boundary message implementations SHOULD provide standard contract lifecycle operations including Reset, String, and Descriptor access.

### Verify

```bash
# Discover and execute the repository contract validation test suite to confirm all generated boundary structures satisfy ProtoMessage and ProtoReflect interfaces.
# Execute the workspace static analysis checks to verify that descriptor initialization conforms to synchronized single-execution patterns without unauthorized memory manipulation.
```

**Accept when:**
- All boundary message contracts satisfy ProtoMessage, ProtoReflect, and Descriptor interface contracts without runtime reflection faults.
- Test suites across all schema-dependent components pass with zero concurrent descriptor initialization panics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated verification checks enforce interface compliance for all boundary payload structures.
</enforcement>