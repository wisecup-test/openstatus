---
version: 2
name: cross-cutting-remote-service-communication-establish-c-a284
description: "Protocol Buffers Contract Architecture with google.golang.org/protobuf: Remote Service Communication Establish Client Connectivity"
metadata:
  actual:
    adr_id: a28465ce-cc47-4472-862c-a646c994a51f
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

# Protocol Buffers Contract Architecture with google.golang.org/protobuf: Remote Service Communication Establish Client Connectivity

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/**`, `apps/checker/handlers/**`, `apps/checker/pkg/**`, `apps/checker/proto/private_location/v1/**`, `apps/checker/request/**`.

These rules are ALWAYS ACTIVE for public and external API boundaries defining monitoring jobs, regional health telemetry, and service contracts, and cross-process communication between coordinator services and distributed checker execution daemons.

### Rules

- **R-PROTO-001** MUST: Remote service communication MUST establish client connectivity through generated client constructors and authenticate via dedicated interceptors including NewAuthInterceptor.

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
Claude Code MUST NOT skip or defer verification. All pull requests and code modifications must adhere strictly to protocol buffer interfaces, and automated build pipelines will fail on schema divergence or unvalidated message structures.
</enforcement>