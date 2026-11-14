---
version: 2
name: cross-cutting-service-endpoints-managing-public-remote-55fd
description: "Protocol Buffers Contract Architecture with google.golang.org/protobuf: Service Endpoints Managing Public Remote Monitoring"
metadata:
  actual:
    adr_id: 55fd487d-1647-4b91-b4fb-465cbdde180a
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

# Protocol Buffers Contract Architecture with google.golang.org/protobuf: Service Endpoints Managing Public Remote Monitoring

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/**`, `apps/checker/handlers/**`, `apps/checker/pkg/**`, `apps/checker/proto/private_location/v1/**`, `apps/checker/request/**`.

These rules are ALWAYS ACTIVE for public and external API boundaries defining monitoring jobs, regional health telemetry, service contracts, and cross-process communication between coordinator services and distributed checker execution daemons.

### Rules

- **R-PROTO-001** MUST_NOT: Service endpoints managing public and remote monitoring contracts MUST NOT accept or emit untyped raw dictionary structures where formal schema definitions exist.

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