---
version: 2
name: cross-cutting-consumer-inspect-repository-dependency-r-51d1
description: "httputil.ReverseProxy Gateway Architecture: Consumer Inspect Repository Dependency Resolution Artifacts"
metadata:
  actual:
    adr_id: 51d1eaca-b920-452b-83ad-07ad1f72c825
    status: accepted
    candidate_group_id: 4f5b9518-22b8-4be4-a515-5b19dc1988c1
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Gateway Architecture: Consumer Inspect Repository Dependency Resolution Artifacts

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for reverse proxy and API gateway services routing inbound traffic to upstream backends, and network perimeter endpoints requiring transport-level dial retry resilience.

### Rules

- **R-REV-001** MUST: The consumer MUST inspect the repository dependency resolution artifacts and verify the locked versions of all external gateway dependencies before implementing proxy routing.

### Verify

```bash
# Discover and run the project's gateway routing and proxy integration test suites
# Discover and execute the repository static analysis and verification scripts to confirm reverse proxy transport configurations
```

**Accept when:**
- Reverse proxy tests verify that upstream target URLs and Host headers are rewritten correctly.
- Transport tests verify that dial failures trigger retry attempts with backoff while non-dial errors fail immediately without retry.
- Context cancellation aborts retry loops without executing subsequent RoundTrip calls.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>