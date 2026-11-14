---
version: 2
name: cross-cutting-proxy-http-transports-wrap-upstream-8964
description: "httputil.ReverseProxy Gateway Architecture: Proxy Http Transports Wrap Upstream Communications"
metadata:
  actual:
    adr_id: 89642419-8ca4-4952-94b7-db2c8162cb10
    status: accepted
    candidate_group_id: 4f5b9518-22b8-4be4-a515-5b19dc1988c1
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Gateway Architecture: Proxy Http Transports Wrap Upstream Communications

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GATEWAY-001** MUST: Proxy HTTP transports MUST wrap upstream communications in a dedicated RoundTrip implementation that buffers incoming request bodies and handles dial failure retries with exponential backoff and context cancellation.

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
Claude Code MUST NOT skip or defer verification. Verified by automated integration test suites executed in continuous integration pipelines and peer code review.
</enforcement>