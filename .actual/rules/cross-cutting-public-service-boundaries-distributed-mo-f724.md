---
version: 2
name: cross-cutting-public-service-boundaries-distributed-mo-f724
description: "Protocol Buffers Contract Architecture with google.golang.org/protobuf: Public Service Boundaries Distributed Monitoring Integration"
metadata:
  actual:
    adr_id: f72498b7-3d4e-4878-a836-5cfcb8e9c58d
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

# Protocol Buffers Contract Architecture with google.golang.org/protobuf: Public Service Boundaries Distributed Monitoring Integration

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/**`, `apps/checker/handlers/**`, `apps/checker/pkg/**`, `apps/checker/proto/private_location/v1/**`, `apps/checker/request/**`.

These rules are ALWAYS ACTIVE for public service boundaries, distributed monitoring integration contracts, and cross-process communication between coordinator services and distributed checker execution daemons.

### Rules

- **R-PROTO-001** MUST: Public service boundaries and distributed monitoring integration contracts MUST be defined using Protocol Buffers schemas implemented through google.golang.org/protobuf message types.

### Verify

```bash
# Discover and execute repository test suites governing external API contracts
# Discover and execute repository schema linters and contract compatibility checks
# Verify lock artifacts to ensure resolved dependency versions match manifest constraints
```

**Accept when:**
- All contract validation tests and protocol buffer message serialization test suites execute successfully without errors.
- Repository dependency verification confirms that the resolved contract library matches the locked artifact version.
- All external service communications and message structures strictly implement required protocol message and reflection interfaces.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>