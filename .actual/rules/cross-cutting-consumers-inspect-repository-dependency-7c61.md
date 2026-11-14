---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-7c61
description: "Direct HTTP Response Body Handling via http.Get: Consumers Inspect Repository Dependency Manifests Lock"
metadata:
  actual:
    adr_id: 7c61dd92-5d3c-4afa-9059-b5c4f7767c45
    status: accepted
    candidate_group_id: e29e2720-9496-4aca-a344-b4e83bda4581
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# Direct HTTP Response Body Handling via http.Get: Consumers Inspect Repository Dependency Manifests Lock

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for service boundary routines that execute outgoing HTTP requests using http.Get and process response body streams.

### Rules

- **R-HTTP-001** MUST: Consumers MUST inspect repository dependency manifests and lock artifacts to resolve exact locked dependency versions before introducing or updating network client dependencies.

### Verify

```bash
# Discover the repository build configuration and execute the standard test suite to validate HTTP client integration behavior.
# Discover static analysis and linting scripts in the project manifest and execute them to verify response body stream resource closure.
```

**Accept when:**
- All integration test targets pass cleanly without requiring active external network connections.
- Static verification confirms that all response body streams are closed across all execution paths.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration test execution and peer code reviews.
</enforcement>