---
version: 2
name: cross-cutting-network-boundary-integrations-communicat-c5c6
description: "Direct HTTP Response Body Handling via http.Get: Network Boundary Integrations Communicating Via Http"
metadata:
  actual:
    adr_id: c5c6e2e3-47a0-41a9-a386-a5698b812fdb
    status: accepted
    candidate_group_id: e29e2720-9496-4aca-a344-b4e83bda4581
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# Direct HTTP Response Body Handling via http.Get: Network Boundary Integrations Communicating Via Http

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for service boundary routines that execute outgoing HTTP requests using http.Get and process response body streams.

### Rules

- **R-NET-001** MUST: Network boundary integrations communicating via http.Get MUST isolate external client calls behind testable boundary interfaces rather than binding logic directly to live response body streams.

### Verify

```bash
# Discover the repository build configuration and execute the standard test suite to validate HTTP client integration behavior.
# Discover static analysis and linting scripts in the project manifest and execute them to verify response body stream resource closure.
```

**Accept when:**
- All integration test targets pass cleanly without requiring active external network connections.
- Static verification confirms that all response body streams are closed across all execution paths.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test execution and peer code reviews enforce these requirements.
</enforcement>