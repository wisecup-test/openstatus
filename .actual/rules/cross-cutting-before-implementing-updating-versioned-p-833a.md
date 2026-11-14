---
version: 2
name: cross-cutting-before-implementing-updating-versioned-p-833a
description: "httputil.ReverseProxy Retrying Transport with Request Body Replay: Before Implementing Updating Versioned Proxy Dependencies"
metadata:
  actual:
    adr_id: 833a128b-9418-4533-b944-7c0f01b6c2ab
    status: accepted
    candidate_group_id: c395309c-fccd-436b-9eed-4e7605867f18
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Retrying Transport with Request Body Replay: Before Implementing Updating Versioned Proxy Dependencies

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REV-001** MUST: Before implementing or updating versioned proxy dependencies, the engineering team MUST discover the repository lock artifact and verify the exact resolved dependency versions.
- **R-REV-002** MUST: Restrict retry eligibility strictly to isDialError to guarantee requests are re-sent only when connection establishment fails prior to upstream request processing.
- **R-REV-003** MUST: Buffer request bodies into memory prior to transport execution to enable deterministic body replay across retry attempts without requiring upstream protocol modifications.
- **R-REV-004** MUST: Integrate backoff waiting with req.Context().Done() to prevent proxy resource leakage and hung execution threads when clients disconnect or timeout during transient network outages.
- **R-REV-005** MUST: Close the incoming request body stream immediately after reading into a byte slice to prevent file descriptor leaks.

### Verify

```bash
# Discover and execute the project unit and integration test suites covering HTTP proxy transport behavior.
# Discover and run the project static analysis and linting checks to ensure proper error handling and context propagation.
```

**Accept when:**
- The custom transport retries requests upon simulated dial failure and delivers upstream response payloads upon successful reconnection.
- The custom transport terminates immediately without retry when encountering non-dial errors or when request context is canceled.
- All discovered automated test suites and static analysis checks pass with zero exit codes.

<enforcement>
Claude Code MUST NOT skip or defer verification. All proxy transport configurations and routing boundaries are subject to automated continuous integration pipelines and peer code review.
</enforcement>