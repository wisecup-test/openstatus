---
version: 2
name: cross-cutting-proxy-transports-not-retry-requests-dd39
description: "httputil.ReverseProxy Gateway Architecture: Proxy Transports Not Retry Requests When"
metadata:
  actual:
    adr_id: dd393fd9-0d6f-4d5a-bcbf-f410d1fa24cd
    status: accepted
    candidate_group_id: 4f5b9518-22b8-4be4-a515-5b19dc1988c1
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Gateway Architecture: Proxy Transports Not Retry Requests When

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for reverse proxy and API gateway services routing inbound traffic to upstream backends, and network perimeter endpoints requiring transport-level dial retry resilience.

### Rules

- **R-PRX-001** MUST_NOT: Proxy transports MUST NOT retry requests when an error is not classified as a dial failure or when request contexts are canceled.

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
Claude Code MUST NOT skip or defer verification. Automated integration test suites and peer code reviews enforce these requirements.
</enforcement>