---
version: 2
name: cross-cutting-telemetry-aggregators-metric-recorders-u-13c2
description: "Protocol Buffers Contract Architecture with google.golang.org/protobuf: Telemetry Aggregators Metric Recorders Utilize Descriptor"
metadata:
  actual:
    adr_id: 13c2795a-8345-4291-8dfd-6da137bcd743
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

# Protocol Buffers Contract Architecture with google.golang.org/protobuf: Telemetry Aggregators Metric Recorders Utilize Descriptor

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/**`, `apps/checker/handlers/**`, `apps/checker/pkg/**`, `apps/checker/proto/private_location/v1/**`, `apps/checker/request/**`.

These rules are ALWAYS ACTIVE for public and external API boundaries defining monitoring jobs, regional health telemetry, service contracts, and cross-process communication between coordinator services and distributed checker execution daemons.

### Rules

- **R-PB-001** MAY: Telemetry aggregators and metric recorders MAY utilize Descriptor introspection for generic metric attribute mapping and dynamic schema inspection.

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
Claude Code MUST NOT skip or defer verification. All contract changes must be validated against continuous integration contract test pipelines and locked dependency versions.
</enforcement>