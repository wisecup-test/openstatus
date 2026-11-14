---
version: 2
name: cross-cutting-retry-transport-not-attempt-request-cc23
description: "httputil.ReverseProxy Retrying Transport with Request Body Replay: Retry Transport Not Attempt Request Replay"
metadata:
  actual:
    adr_id: cc23136b-fc9e-43c7-82fd-fe7d699920e2
    status: accepted
    candidate_group_id: c395309c-fccd-436b-9eed-4e7605867f18
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Retrying Transport with Request Body Replay: Retry Transport Not Attempt Request Replay

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRANS-001** MUST_NOT: The retry transport MUST NOT attempt request replay if initial request body reading fails or if the request body stream cannot be buffered.

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