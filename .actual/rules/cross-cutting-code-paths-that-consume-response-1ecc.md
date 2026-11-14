---
version: 2
name: cross-cutting-code-paths-that-consume-response-1ecc
description: "Direct HTTP Response Body Handling via http.Get: Code Paths That Consume Response Body"
metadata:
  actual:
    adr_id: 1ecce091-ce0a-41ac-9f0a-33e4822ecee1
    status: accepted
    candidate_group_id: e29e2720-9496-4aca-a344-b4e83bda4581
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# Direct HTTP Response Body Handling via http.Get: Code Paths That Consume Response Body

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for service boundary routines that execute outgoing HTTP requests using http.Get and process response body streams.

### Rules

- **R-HTTP-001** MUST: Code paths that consume response body streams MUST ensure underlying resources are closed reliably across all execution branches.

### Verify

```bash
# Discover the repository build configuration and execute the standard test suite to validate HTTP client integration behavior.
# Discover static analysis and linting scripts in the project manifest and execute them to verify response body stream resource closure.
go test ./...
```

**Accept when:**
- All integration test targets pass cleanly without requiring active external network connections.
- Static verification confirms that all response body streams are closed across all execution paths.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test execution and peer code reviews enforce compliance.
</enforcement>