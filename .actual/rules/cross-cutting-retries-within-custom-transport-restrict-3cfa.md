---
version: 2
name: cross-cutting-retries-within-custom-transport-restrict-3cfa
description: "httputil.ReverseProxy Retrying Transport with Request Body Replay: Retries Within Custom Transport Restricted Strictly"
metadata:
  actual:
    adr_id: 3cfa5f3a-4e28-4b85-aa74-31d972dc0cc1
    status: accepted
    candidate_group_id: c395309c-fccd-436b-9eed-4e7605867f18
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Retrying Transport with Request Body Replay: Retries Within Custom Transport Restricted Strictly

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HTTP-001** MUST: Retries within the custom transport MUST be restricted strictly to dial failures verified by isDialError, immediately aborting and returning non-dial transport or application errors.

### Verify

```bash
# Discover and execute the project unit and integration test suites covering HTTP proxy transport behavior
# Discover and run the project static analysis and linting checks to ensure proper error handling and context propagation
```

**Accept when:**
- The custom transport retries requests upon simulated dial failure and delivers upstream response payloads upon successful reconnection.
- The custom transport terminates immediately without retry when encountering non-dial errors or when request context is canceled.
- All discovered automated test suites and static analysis checks pass with zero exit codes.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>