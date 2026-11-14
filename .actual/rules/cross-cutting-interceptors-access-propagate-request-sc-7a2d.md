---
version: 2
name: cross-cutting-interceptors-access-propagate-request-sc-7a2d
description: "@connectrpc/connect Interceptor Pipeline Architecture: Interceptors Access Propagate Request Scoped Metadata"
metadata:
  actual:
    adr_id: 7a2de9dd-f944-4d3b-9c13-c09f9dc748fc
    status: accepted
    candidate_group_id: 4d15d7b7-58ac-4c28-ae26-f4b6181018d4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/interceptors/**
---

# @connectrpc/connect Interceptor Pipeline Architecture: Interceptors Access Propagate Request Scoped Metadata

Scope: applies to `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all inbound and outbound RPC service procedures handled through the @connectrpc/connect server routing layer and cross-cutting request lifecycle handlers.

### Rules

- **R-CI-001** MUST: Interceptors MUST access and propagate request-scoped metadata exclusively through @connectrpc/connect request context values using strongly typed context keys.

### Verify

```bash
# Discover and run project static analysis, type checking, and automated test suites
# (Commands derived from repository manifests per discovery policy)
```

**Accept when:**
- Static type analysis verifies that all interceptors strictly conform to @connectrpc/connect interceptor signatures without type errors
- Automated test suites covering RPC interceptors pass successfully, verifying that logging, error handling, and context propagation execute as specified

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and mandatory architectural peer reviews.
</enforcement>