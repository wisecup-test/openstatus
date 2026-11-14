---
version: 2
name: cross-cutting-route-handlers-isolate-domain-business-6fa1
description: "Adoption of next/server for Server Route and Request Handling: Route Handlers Isolate Domain Business Logic"
metadata:
  actual:
    adr_id: 6fa13c08-f2af-4229-86f6-f3982d6d88e3
    status: accepted
    candidate_group_id: 2dd81b33-c7f2-44a9-87ad-46fa4c700394
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of next/server for Server Route and Request Handling: Route Handlers Isolate Domain Business Logic

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all server-side HTTP route handlers, proxy dispatchers, webhook receivers, and identity provider callback endpoints within the application.

### Rules

- **R-ADR-001** SHOULD: Route handlers SHOULD isolate domain business logic and database interactions into designated internal service modules, keeping transport-level request extraction within the server module boundary.
- **R-ADR-002** MANDATORY: DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-ADR-003** MANDATORY: LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
  1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
  2. Identify the build tool from the manifest.
  3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
  4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
  5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
  6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-ADR-004** MANDATORY: Extract request headers and query parameters at the handler boundary and map them to domain values before invoking internal services.
- **R-ADR-005** MANDATORY: Configure cookie security attributes, including path, expiration, and transport security flags, consistently using the server response cookie API.

### Verify

```bash
# Discover the project test runner from repository configuration and execute the server test suite.
# Discover the project type checker from repository configuration and verify type compliance across server route handlers.
# Discover the project linter from repository configuration and execute static analysis across server modules.
```

**Accept when:**
- All server route handlers and proxy modules compile without type errors using the server module interfaces.
- HTTP endpoint test suites pass successfully, verifying header extraction and cookie lifecycle management.
- Static analysis confirms no unauthorized direct transport mutations or unsupported runtime socket access in server route handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance is verified through continuous integration automated testing, static analysis, and peer review.
</enforcement>