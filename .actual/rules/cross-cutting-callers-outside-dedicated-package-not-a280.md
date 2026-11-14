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

- **R-AI-001** MUST_NOT: Callers outside the dedicated AI package MUST NOT import @ai-sdk/openai-compatible directly and MUST instead consume resolved model instances through the unified AI interface.

### Verify

```bash
# Discover package manager / test runner and execute tests
if [ -f "package.json" ]; then
  npm test || pnpm test || yarn test
fi

# Discover and run static analysis / type checking
if [ -f "package.json" ]; then
  npm run check || npm run lint || npm run typecheck || pnpm check || pnpm lint || pnpm typecheck || yarn check || yarn lint || yarn typecheck
fi
```

**Accept when:**
- The automated test suite passes with all OpenAI-compatible provider resolution tests asserting valid language model instance instantiation.
- Static type analysis confirms zero type errors across module boundaries importing and consuming the model resolution exports.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline runs automated tests and static type checking across the AI module, and pull requests containing direct imports of @ai-sdk/openai-compatible outside the model resolution layer will be blocked from merging.
</enforcement>