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
- **R-STD-003** LOCK-VERSION GROUNDING (MANDATORY): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact for exact resolved version, (4) Look up official docs/changelog for that exact version, (5) Confirm every API/class/function exists in that version, (6) Re-run steps 3-5 per dependency at point of use.
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
Claude Code MUST NOT skip or defer verification. Automated dependency audit and peer review verification during code changes in test utility modules govern enforcement. Violation handling requires flagging unapproved assertion libraries during code review and pipeline checks, and requiring migration of non-conforming matchers to the approved expectation library before merging. Exceptions require an approved architectural variance request detailing technical incompatibility with jsr:@std/expect.
</enforcement>