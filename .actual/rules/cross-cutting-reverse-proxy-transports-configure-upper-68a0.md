---
version: 2
name: cross-cutting-reverse-proxy-transports-configure-upper-68a0
description: "httputil.ReverseProxy Retrying Transport with Request Body Replay: Reverse Proxy Transports Configure Upper Bound"
metadata:
  actual:
    adr_id: 68a02702-4e2b-4fb1-8bb4-d55fc13abda7
    status: accepted
    candidate_group_id: c395309c-fccd-436b-9eed-4e7605867f18
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Retrying Transport with Request Body Replay: Reverse Proxy Transports Configure Upper Bound

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROXY-001** SHOULD: Reverse proxy transports SHOULD configure an upper bound on retry attempts and enforce backoff duration between successive connection attempts.
- **R-PROXY-002** MANDATORY: The consumer MUST discover dependency manifests, build tools, exact resolved versions from lock/resolution artifacts, and public API references before writing code that uses a versioned library.
- **R-PROXY-003** MANDATORY: Implementations wrapping httputil.ReverseProxy must assign the custom retry transport to the Transport field while ensuring rewrite or director handlers preserve target host headers.
- **R-PROXY-004** MANDATORY: The incoming request body stream must be closed immediately after reading into a byte slice to prevent file descriptor leaks.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipeline running proxy integration and transport unit tests, and peer code review verification of all reverse proxy transport configurations and routing boundaries, are mandatory.
</enforcement>