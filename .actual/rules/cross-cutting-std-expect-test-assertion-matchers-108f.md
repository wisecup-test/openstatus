---
version: 2
name: cross-cutting-std-expect-test-assertion-matchers-108f
description: "Adopt jsr:@std/expect for Test Assertion Matchers: Shared Assertion Matchers Centralized Within Dedicated"
metadata:
  actual:
    adr_id: 108f70b8-0f68-4dd3-b33c-028e616d11ca
    status: accepted
    candidate_group_id: a309ace2-0aca-4547-bae3-648a49462cd1
    evidence:
      source: deep
  scope:
    root: packages/test-utils
    paths:
      - src/**
---

# Adopt jsr:@std/expect for Test Assertion Matchers: Shared Assertion Matchers Centralized Within Dedicated

Scope: applies to `packages/test-utils/src/**`.

These rules are ALWAYS ACTIVE for test utility modules authoring custom assertion matchers and test suites consuming shared assertion expectation helpers.

### Rules

- **R-STD-001** SHOULD: Shared assertion matchers SHOULD be centralized within dedicated test utility modules rather than redefined across individual test suites.
- **R-STD-002** MANDATORY: Discover the project repository's test runner, dependency manifest, and lock artifact to derive tools, file names, commands, package managers, and exact resolved library versions.
- **R-STD-003** MANDATORY: Ground dependency versions by inspecting the repository lock or resolution artifact, fetching official documentation/API references for that exact version, and confirming API availability before writing code.
- **R-STD-004** MANDATORY: Export custom matchers through shared test utility modules to encapsulate assertion implementation details from individual test suites.
- **R-STD-005** MANDATORY: Ensure all custom assertion matchers conform to the expectation interface contract established by the resolved library version.

### Verify

```bash
echo "Discover repository test runner and execute assertion test suites"
command -v test >/dev/null 2>&1 && echo "Verify test execution using repository build configuration" || echo "Discover and execute test runner from project manifest"
```

**Accept when:**
- All assertion matchers defined using jsr:@std/expect execute and pass within the repository test runner
- No conflicting assertion expectation libraries are declared within the project dependency manifest or lock artifact

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>