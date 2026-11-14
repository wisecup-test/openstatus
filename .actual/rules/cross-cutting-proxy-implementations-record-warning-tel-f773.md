---
version: 2
name: cross-cutting-proxy-implementations-record-warning-tel-f773
description: "httputil.ReverseProxy Retrying Transport with Request Body Replay: Proxy Implementations Record Warning Telemetry Attempt"
metadata:
  actual:
    adr_id: f7735705-8d6f-4850-82fe-8344918160fd
    status: accepted
    candidate_group_id: c395309c-fccd-436b-9eed-4e7605867f18
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Retrying Transport with Request Body Replay: Proxy Implementations Record Warning Telemetry Attempt

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PRX-001** MAY: Proxy implementations MAY record warning telemetry with attempt counts and destination target strings for failed dial attempts before re-dispatching.

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
Claude Code MUST NOT skip or defer verification. All proxy implementations must be verified through automated continuous integration testing, static analysis checks, and peer code review.
</enforcement>