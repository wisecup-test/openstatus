---
version: 2
name: cross-cutting-callers-outside-dedicated-package-not-a280
description: "Adoption of @ai-sdk/openai-compatible for Model Provider Resolution: Callers Outside Dedicated Package Not Import"
metadata:
  actual:
    adr_id: a2805c76-2859-4037-8f6a-1b4411b4a506
    status: accepted
    candidate_group_id: c5f02e5f-768e-49c4-9e06-66844f1be5cb
    evidence:
      source: deep
  scope:
    root: packages/ai
    paths:
      - src/**
---

# Adoption of @ai-sdk/openai-compatible for Model Provider Resolution: Callers Outside Dedicated Package Not Import

Scope: applies to `packages/ai/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MOD-001** MUST_NOT: Callers outside the dedicated AI package MUST NOT import @ai-sdk/openai-compatible directly and MUST instead consume resolved model instances through the unified AI interface.

### Verify

```bash
# Discover the repository package manifest and execute the configured test suite to verify model resolution behavior across configured provider endpoints.
# Discover and run the project static analysis and type checking scripts to verify contract conformity for all provider factory invocations.
```

**Accept when:**
- The automated test suite passes with all OpenAI-compatible provider resolution tests asserting valid language model instance instantiation.
- Static type analysis confirms zero type errors across module boundaries importing and consuming the model resolution exports.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>