---
version: 2
name: cross-cutting-route-handlers-construct-responses-handl-dc73
description: "Adoption of next/server for Server-Side HTTP and Proxy Handling: Route Handlers Construct Responses Handle Redirect"
metadata:
  actual:
    adr_id: dc738ab6-25bb-4147-95ae-57e9edfa22cd
    status: accepted
    candidate_group_id: a8fe49ef-a92a-4310-8b4c-0d125ad33f95
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of next/server for Server-Side HTTP and Proxy Handling: Route Handlers Construct Responses Handle Redirect

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NSV-001** SHOULD: Route handlers SHOULD construct responses and handle redirect or rewrite logic using NextResponse utilities provided by next/server to maintain consistent header manipulation and cookie assignment.
- **R-NSV-002** MANDATORY: Ensure route handlers import server primitives exclusively from next/server and adhere to supported handler function signatures for HTTP methods.
- **R-NSV-003** MANDATORY: Utilize next/server response helpers for status code assignment, header mutation, and cookie writing to ensure downstream proxy middleware compatibility.
- **R-NSV-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect lock/resolution artifact for exact version, (4) Look up official docs for that exact version, (5) Confirm every API/class/function exists in that version's docs, (6) Re-run per dependency at point of use.

### Verify

```bash
# Discover the repository dependency manifest and run the declared type checking script to verify next/server import compliance.
# Discover and execute the repository test runner script targeting route handlers and proxy modules to validate server response contracts.
# Discover and run the project linting script to ensure no unauthorized HTTP response primitives are imported in server-side entry points.
```

**Accept when:**
- All server route handlers and proxy modules compile successfully using next/server types without type errors.
- Automated test suites for route handlers, badges, and proxy redirection pass with zero regressions.
- No deprecated or unsupported server-side response methods are invoked across server endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing repository type verification and test suites, and peer code review.
</enforcement>