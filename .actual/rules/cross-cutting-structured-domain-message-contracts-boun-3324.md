---
version: 2
name: cross-cutting-structured-domain-message-contracts-boun-3324
description: "Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Structured Domain Message Contracts Boundary Communication"
metadata:
  actual:
    adr_id: 332409e9-1a1e-4cc8-b527-813e07c3c643
    status: accepted
    candidate_group_id: f13253cc-8873-43d0-88ed-e6192e7e6c56
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Structured Domain Message Contracts Boundary Communication

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all structured domain models, client communication contracts, and monitoring specifications requiring serialization or schema reflection.

### Rules

- **R-PROT-001** MUST: All structured domain message contracts and boundary communication schemas MUST be implemented using the google.golang.org/protobuf runtime and reflection modules.
- **R-PROT-002** MUST: Ensure all message types are generated with proper synchronization primitives around descriptor data decompression to prevent multi-threaded data races.
- **R-PROT-003** MUST: Maintain strict separation between generated protocol buffer contracts and domain logic by wrapping message instances with domain adapters where appropriate.
- **R-PROT-004** MUST: Execute lock-version grounding in order (discover dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation, confirm API existence, re-run per dependency at point of use) prior to writing code that uses a versioned library.

### Verify

```bash
# Discover and execute the repository contract validation test suite to confirm all generated boundary structures satisfy ProtoMessage and ProtoReflect interfaces.
# Execute the workspace static analysis checks to verify that descriptor initialization conforms to synchronized single-execution patterns without unauthorized memory manipulation.
```

**Accept when:**
- All boundary message contracts satisfy ProtoMessage, ProtoReflect, and Descriptor interface contracts without runtime reflection faults.
- Test suites across all schema-dependent components pass with zero concurrent descriptor initialization panics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated repository verification checks enforce interface compliance for all boundary payload structures.
</enforcement>