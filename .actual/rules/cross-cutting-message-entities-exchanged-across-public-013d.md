---
version: 2
name: cross-cutting-message-entities-exchanged-across-public-013d
description: "Protocol Buffers Contract Architecture with google.golang.org/protobuf: Message Entities Exchanged Across Public Boundaries"
metadata:
  actual:
    adr_id: 013dbcf2-27ec-4d6c-8c44-c7c2c7b6a6ef
    status: accepted
    candidate_group_id: 64d684e7-b04a-42f9-82f0-1a520177c4fd
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/**
      - handlers/**
      - pkg/**
      - proto/private_location/v1/**
      - request/**
---

# Protocol Buffers Contract Architecture with google.golang.org/protobuf: Message Entities Exchanged Across Public Boundaries

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/**`, `apps/checker/handlers/**`, `apps/checker/pkg/**`, `apps/checker/proto/private_location/v1/**`, `apps/checker/request/**`.

These rules are ALWAYS ACTIVE for public and external API boundaries defining monitoring jobs, regional health telemetry, and service contracts, and cross-process communication between coordinator services and distributed checker execution daemons.

### Rules

- **R-PB-001** MUST: All message entities exchanged across public API boundaries MUST implement ProtoMessage and ProtoReflect interfaces.

### Verify

```bash
echo "Discover and execute repository test suites governing external API contracts"
echo "Discover and execute repository schema linters and contract compatibility checks"
echo "Verify lock artifacts to ensure resolved dependency versions match manifest constraints"
```

**Accept when:**
- All contract validation tests and protocol buffer message serialization test suites execute successfully without errors.
- Repository dependency verification confirms that the resolved contract library matches the locked artifact version.
- All external service communications and message structures strictly implement required protocol message and reflection interfaces.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>