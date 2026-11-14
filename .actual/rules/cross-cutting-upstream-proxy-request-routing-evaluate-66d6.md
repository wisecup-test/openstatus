---
version: 2
name: cross-cutting-upstream-proxy-request-routing-evaluate-66d6
description: "httputil.ReverseProxy Gateway Architecture: Upstream Proxy Request Routing Evaluate Regional"
metadata:
  actual:
    adr_id: 66d677b4-1929-425a-a8f5-620ca39a5de6
    status: accepted
    candidate_group_id: 4f5b9518-22b8-4be4-a515-5b19dc1988c1
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Gateway Architecture: Upstream Proxy Request Routing Evaluate Regional

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REV-001** SHOULD: Upstream proxy request routing SHOULD evaluate regional routing headers such as railway-region before rewriting request destinations.

### Verify

```bash
# Discover and run the project's gateway routing and proxy integration test suites.
# Discover and execute the repository static analysis and verification scripts to confirm reverse proxy transport configurations.
```

**Accept when:**
- Reverse proxy tests verify that upstream target URLs and Host headers are rewritten correctly.
- Transport tests verify that dial failures trigger retry attempts with backoff while non-dial errors fail immediately without retry.
- Context cancellation aborts retry loops without executing subsequent RoundTrip calls.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>