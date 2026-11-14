---
version: 2
name: cross-cutting-inbound-service-endpoints-health-checks-549d
description: "httputil.ReverseProxy Gateway Architecture: Inbound Service Endpoints Health Checks Coordinated"
metadata:
  actual:
    adr_id: 549daa86-ca94-4cb0-bd99-e7ea9a012e3d
    status: accepted
    candidate_group_id: 4f5b9518-22b8-4be4-a515-5b19dc1988c1
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Gateway Architecture: Inbound Service Endpoints Health Checks Coordinated

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GATEWAY-001** SHOULD: Inbound service endpoints and health checks SHOULD be coordinated using gin.Context handlers separated from proxy forwarding pathways.
- **R-GATEWAY-002** MANDATORY: Ensure request body readers are reset using buffer readers prior to each retry iteration within RoundTrip.
- **R-GATEWAY-003** MANDATORY: Propagate context cancellation checks prior to initiating backoff pauses to prevent lingering goroutines during client disconnects.

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
Claude Code MUST NOT skip or defer verification. Automated integration test suites executed in continuous integration pipelines and peer code review enforce compliance.
</enforcement>