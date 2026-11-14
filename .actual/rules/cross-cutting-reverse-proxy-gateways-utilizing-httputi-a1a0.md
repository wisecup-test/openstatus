---
version: 2
name: cross-cutting-reverse-proxy-gateways-utilizing-httputi-a1a0
description: "httputil.ReverseProxy Retrying Transport with Request Body Replay: Reverse Proxy Gateways Utilizing Httputil Reverseproxy"
metadata:
  actual:
    adr_id: a1a04213-5ade-4a57-803d-ab5286b300a4
    status: accepted
    candidate_group_id: c395309c-fccd-436b-9eed-4e7605867f18
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Retrying Transport with Request Body Replay: Reverse Proxy Gateways Utilizing Httputil Reverseproxy

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REVPROXY-001** MUST: All reverse proxy gateways utilizing httputil.ReverseProxy for outbound traffic MUST execute upstream dispatch through a custom http.RoundTripper that buffers request bodies for retry replay.

### Verify

```bash
# Discover and execute project unit and integration test suites covering HTTP proxy transport behavior.
# Discover and run project static analysis and linting checks to ensure proper error handling and context propagation.
```

**Accept when:**
- The custom transport retries requests upon simulated dial failure and delivers upstream response payloads upon successful reconnection.
- The custom transport terminates immediately without retry when encountering non-dial errors or when request context is canceled.
- All discovered automated test suites and static analysis checks pass with zero exit codes.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>