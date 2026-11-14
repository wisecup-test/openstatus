---
version: 2
name: cross-cutting-domain-job-runners-map-protocol-2a51
description: "Protocol Buffers Contract Architecture with google.golang.org/protobuf: Domain Job Runners Map Protocol Buffer"
metadata:
  actual:
    adr_id: 2a51cde8-be3a-4d75-96d9-dc3533563fb4
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

# Protocol Buffers Contract Architecture with google.golang.org/protobuf: Domain Job Runners Map Protocol Buffer

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/**`, `apps/checker/handlers/**`, `apps/checker/pkg/**`, `apps/checker/proto/private_location/v1/**`, `apps/checker/request/**`.

These rules are ALWAYS ACTIVE for public and external API boundaries defining monitoring jobs, regional health telemetry, and service contracts, and cross-process communication between coordinator services and distributed checker execution daemons.

### Rules

- **R-PB-001** SHOULD: Domain job runners SHOULD map protocol buffer assertion records to execution logic using specialized comparators such as ProtoRecordAssertionToComparator.

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
Claude Code MUST NOT skip or defer verification. Continuous integration contract test pipelines and pull request code reviews enforce these requirements.
</enforcement>