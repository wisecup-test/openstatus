---
version: 2
name: cross-cutting-reverse-proxy-service-implement-forwardi-eff4
description: "httputil.ReverseProxy Gateway Architecture: Reverse Proxy Service Implement Forwarding Boundaries"
metadata:
  actual:
    adr_id: eff45b05-bc0a-4f0a-a9dd-f4dc5e34870a
    status: accepted
    candidate_group_id: 4f5b9518-22b8-4be4-a515-5b19dc1988c1
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Gateway Architecture: Reverse Proxy Service Implement Forwarding Boundaries

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for reverse proxy and API gateway services routing inbound traffic to upstream backends, and network perimeter endpoints requiring transport-level dial retry resilience.

### Rules

- **R-REV-001** MUST: The reverse proxy service MUST implement proxy forwarding boundaries using httputil.ReverseProxy with an explicit Rewrite function that configures the target URL and host header.

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