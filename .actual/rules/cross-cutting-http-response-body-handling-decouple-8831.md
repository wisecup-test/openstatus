---
version: 2
name: cross-cutting-http-response-body-handling-decouple-8831
description: "Direct HTTP Response Body Handling via http.Get: Http Response Body Handling Decouple Stream"
metadata:
  actual:
    adr_id: 88316d46-5f3c-4d19-a174-9ed894fa93ab
    status: accepted
    candidate_group_id: e29e2720-9496-4aca-a344-b4e83bda4581
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# Direct HTTP Response Body Handling via http.Get: Http Response Body Handling Decouple Stream

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for service boundary routines that execute outgoing HTTP requests using http.Get and process response body streams.

### Rules

- **R-HTTP-001** SHOULD: HTTP response body handling SHOULD decouple stream decoding and payload deserialization from the underlying transport connection.

### Verify

```bash
# Discover the repository build configuration and execute the standard test suite to validate HTTP client integration behavior.
# Discover static analysis and linting scripts in the project manifest and execute them to verify response body stream resource closure.
```

**Accept when:**
- All integration test targets pass cleanly without requiring active external network connections.
- Static verification confirms that all response body streams are closed across all execution paths.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>