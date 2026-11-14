---
version: 2
name: cross-cutting-custom-transport-roundtrip-method-read-3dbf
description: "httputil.ReverseProxy Retrying Transport with Request Body Replay: Custom Transport Roundtrip Method Read Buffer"
metadata:
  actual:
    adr_id: 3dbf3ce7-4bfc-46ec-ba15-ae3683443667
    status: accepted
    candidate_group_id: c395309c-fccd-436b-9eed-4e7605867f18
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Retrying Transport with Request Body Replay: Custom Transport Roundtrip Method Read Buffer

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RP-001** MUST: The custom transport RoundTrip method MUST read and buffer non-nil request bodies into memory prior to initiating the dispatch loop, re-creating req.Body using io.NopCloser on every retry attempt.

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
Claude Code MUST NOT skip or defer verification. Compliance is validated by the continuous integration pipeline and peer code review.
</enforcement>