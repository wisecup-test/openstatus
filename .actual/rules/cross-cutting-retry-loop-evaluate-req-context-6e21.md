---
version: 2
name: cross-cutting-retry-loop-evaluate-req-context-6e21
description: "httputil.ReverseProxy Retrying Transport with Request Body Replay: Retry Loop Evaluate Req Context Done"
metadata:
  actual:
    adr_id: 6e21f302-7c56-4345-a620-f05c6ffe27d0
    status: accepted
    candidate_group_id: c395309c-fccd-436b-9eed-4e7605867f18
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Retrying Transport with Request Body Replay: Retry Loop Evaluate Req Context Done

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for reverse proxy services utilizing httputil.ReverseProxy and network gateway components requiring resilient connection establishment.

### Rules

- **R-RP-001** MUST: The retry loop MUST evaluate req.Context().Done() during inter-attempt backoff periods and terminate immediately if the client context is canceled or deadline exceeded.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>