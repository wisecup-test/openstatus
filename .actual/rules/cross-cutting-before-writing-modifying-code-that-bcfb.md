---
version: 2
name: cross-cutting-before-writing-modifying-code-that-bcfb
description: "github.com/rs/zerolog Structured and Contextual Logging Adoption: Before Writing Modifying Code That Uses"
metadata:
  actual:
    adr_id: bcfb641a-ebb5-456b-ac01-93786a21892b
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

# github.com/rs/zerolog Structured and Contextual Logging Adoption: Before Writing Modifying Code That Uses

Scope: applies to `apps/checker/checker/**`, `apps/checker/pkg/logger/**`, `apps/checker/pkg/otel/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** MUST: Before writing or modifying code that uses github.com/rs/zerolog, the consumer MUST inspect the repository lock or resolution artifact to determine the exact resolved dependency version and verify API compatibility against official version documentation.

### Verify

```bash
# Discover the repository static analysis and linting scripts from project configuration and run them
# Discover and execute the automated test runner to ensure all logging and telemetry contracts validate successfully
```

**Accept when:**
- Automated test and static analysis suites pass without unformatted standard stream log emissions.
- All service and telemetry logging invocations resolve through github.com/rs/zerolog interfaces.
- Dependency lock verification confirms that the resolved version matches the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>