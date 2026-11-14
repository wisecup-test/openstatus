---
version: 2
name: cross-cutting-std-expect-test-assertion-matchers-a2c5
description: "Adopt jsr:@std/expect for Test Assertion Matchers: Test Utility Matcher Modules Adopt Jsr"
metadata:
  actual:
    adr_id: a2c54584-39da-4263-8759-b526f58098ae
    status: accepted
    candidate_group_id: a309ace2-0aca-4547-bae3-648a49462cd1
    evidence:
      source: deep
  scope:
    root: packages/test-utils
    paths:
      - src/**
---

# Adopt jsr:@std/expect for Test Assertion Matchers: Test Utility Matcher Modules Adopt Jsr

Scope: applies to `packages/test-utils/src/**`.

These rules are ALWAYS ACTIVE for test utility modules authoring custom assertion matchers and test suites consuming shared assertion expectation helpers.

### Rules

- **R-AST-001** MUST: Test utility matcher modules MUST adopt jsr:@std/expect for assertion expectations.

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