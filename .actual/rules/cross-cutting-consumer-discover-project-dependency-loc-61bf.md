---
version: 2
name: cross-cutting-consumer-discover-project-dependency-loc-61bf
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Consumer Discover Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 61bf9f3d-99d5-4b05-92f6-2fb9fe1b5271
    status: accepted
    candidate_group_id: a0de90ee-f110-4e44-a829-5105eaf11733
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Consumer Discover Project Dependency Lock Artifact

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for service boundary contracts, monitoring definitions, and telemetry data models requiring structured serialization, and components exchanging typed message payloads across checker subsystems.

### Rules

- **R-PROTO-001** MUST: The consumer MUST discover the project dependency lock artifact and verify the exact resolved version of google.golang.org/protobuf before introducing or modifying schema implementations.
- **R-PROTO-002** MUST: Execute the LOCK-VERSION GROUNDING procedure in order (find dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for exact version, confirm every API/class/function exists in that version, re-run for version-sensitive behavior at point of use) before writing code that uses a versioned library.
- **R-PROTO-003** MUST: Ensure all schema descriptors are registered through the runtime package to support reflection-based payload inspection and debugging.
- **R-PROTO-004** MUST: Enforce synchronized one-time initialization blocks for raw descriptor decompression across all message schemas to prevent race conditions during concurrent access.

### Verify

```bash
echo "Discover and execute project verification test suites from repository configuration"
echo "Verify that project dependency lock records contain the resolved protobuf runtime dependency"
```

**Accept when:**
- All message definitions compile cleanly and satisfy ProtoMessage and ProtoReflect interfaces.
- Descriptor initialization executes successfully without concurrent race conditions in test suites.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>