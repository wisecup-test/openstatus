---
version: 2
name: cross-cutting-application-packages-not-emit-diagnostic-fe94
description: "github.com/rs/zerolog Structured and Contextual Logging Adoption: Application Packages Not Emit Diagnostic Logs"
metadata:
  actual:
    adr_id: fe941ba3-f566-4d63-9b4e-781e621a6abf
    status: accepted
    candidate_group_id: 69914c3b-8787-467a-9643-10591b9432bc
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - pkg/logger/**
      - pkg/otel/**
---

# github.com/rs/zerolog Structured and Contextual Logging Adoption: Application Packages Not Emit Diagnostic Logs

Scope: applies to `apps/checker/checker/**`, `apps/checker/pkg/logger/**`, `apps/checker/pkg/otel/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZLOG-001** MUST_NOT: Application packages MUST_NOT emit diagnostic logs through unstructured standard output or standard error streams.
- **R-ZLOG-002** MANDATORY: Execute lock-version grounding (discover dependency manifest, identify build tool, inspect repository lock or resolution artifact, look up official documentation for that exact version, confirm API existence, re-verify per dependency at point of use) before writing code that uses a versioned library.
- **R-ZLOG-003** MANDATORY: Invoke the centralized Configure routine during application startup to establish baseline logging settings before serving requests.
- **R-ZLOG-004** MANDATORY: Extract contextual loggers within service handlers using log.Ctx(ctx) to attach relevant diagnostic fields to execution errors.

### Verify

```bash
# Discover the repository static analysis and linting scripts from project configuration and run them to verify logger usage compliance.
# Discover and execute the automated test runner to ensure all logging and telemetry contracts validate successfully.
```

**Accept when:**
- Automated test and static analysis suites pass without unformatted standard stream log emissions.
- All service and telemetry logging invocations resolve through github.com/rs/zerolog interfaces.
- Dependency lock verification confirms that the resolved version matches the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated linting, static analysis rules executed during CI, and mandatory peer review.
</enforcement>