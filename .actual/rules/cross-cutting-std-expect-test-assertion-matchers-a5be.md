---
version: 2
name: cross-cutting-std-expect-test-assertion-matchers-a5be
description: "Adopt jsr:@std/expect for Test Assertion Matchers: Consumer Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: a5be3dd1-e7e0-4d8e-b01b-c3c6a6fc8e0e
    status: accepted
    candidate_group_id: a309ace2-0aca-4547-bae3-648a49462cd1
    evidence:
      source: deep
  scope:
    root: packages/test-utils
    paths:
      - src/**
---

# Adopt jsr:@std/expect for Test Assertion Matchers: Consumer Inspect Repository Lock Artifact Determine

Scope: applies to `packages/test-utils/src/**`.

These rules are ALWAYS ACTIVE for test utility modules authoring custom assertion matchers and test suites consuming shared assertion expectation helpers.

### Rules

- **R-STD-001** MUST: The consumer MUST inspect the repository lock artifact to determine the exact resolved version of jsr:@std/expect before implementing or modifying assertion matchers.
- **R-STD-002** DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-STD-003** LOCK-VERSION GROUNDING (MANDATORY): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. It declares ranges, not installed versions. (2) Identify the build tool from the manifest. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it. (4) Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs. (5) Confirm every API, class, or function you will call exists in that exact version's documentation before using it. (6) For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-STD-004** MUST: Export custom matchers through shared test utility modules to encapsulate assertion implementation details from individual test suites.
- **R-STD-005** MUST: Ensure all custom assertion matchers conform to the expectation interface contract established by the resolved library version.

### Verify

```bash
echo "Discover repository test runner and execute assertion test suites"
command -v test >/dev/null 2>&1 && echo "Verify test execution using repository build configuration" || echo "Discover and execute test runner from project manifest"
```

**Accept when:**
- All assertion matchers defined using jsr:@std/expect execute and pass within the repository test runner
- No conflicting assertion expectation libraries are declared within the project dependency manifest or lock artifact

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated dependency audit of manifest and lock artifacts during integration pipelines, and peer review verification during code changes in test utility modules.
</enforcement>