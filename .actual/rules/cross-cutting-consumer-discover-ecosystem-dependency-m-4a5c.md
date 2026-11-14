---
version: 2
name: cross-cutting-consumer-discover-ecosystem-dependency-m-4a5c
description: "Protocol Buffers Contract Architecture with google.golang.org/protobuf: Consumer Discover Ecosystem Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 4a5c5640-ece7-49df-8c5d-596310f34570
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

# Protocol Buffers Contract Architecture with google.golang.org/protobuf: Consumer Discover Ecosystem Dependency Manifest Lock

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/**`, `apps/checker/handlers/**`, `apps/checker/pkg/**`, `apps/checker/proto/private_location/v1/**`, `apps/checker/request/**`.

These rules are ALWAYS ACTIVE for public and external API boundaries defining monitoring jobs, regional health telemetry, and service contracts, and cross-process communication between coordinator services and distributed checker execution daemons.

### Rules

- **R-PB-001** MUST: The consumer MUST discover the ecosystem dependency manifest and lock artifact in the project repository to identify and validate the exact resolved version of google.golang.org/protobuf prior to implementation.

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