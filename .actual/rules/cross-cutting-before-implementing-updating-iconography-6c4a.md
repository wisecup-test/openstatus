---
version: 2
name: cross-cutting-before-implementing-updating-iconography-6c4a
description: "lucide-react Icon Library Adoption: Before Implementing Updating Iconography Components Consumer"
metadata:
  actual:
    adr_id: 6c4a1d2f-63e9-4dd7-81e4-402fc09856f0
    status: accepted
    candidate_group_id: f22ce03f-373f-4801-9d55-fb8cd1a6ebfb
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
---

# lucide-react Icon Library Adoption: Before Implementing Updating Iconography Components Consumer

Scope: applies to `packages/ui/src/components/**`.

These rules are ALWAYS ACTIVE for all UI component primitives and composite status blocks within the design system package requiring vector iconography.

### Rules

- **R-LUCIDE-001** MUST: Before implementing or updating iconography components, the consumer MUST inspect the repository dependency manifest and lock artifact to resolve and verify the exact locked version of lucide-react.
- **R-LUCIDE-002** MUST: Consumers MUST import required icon symbols directly via named imports from lucide-react to ensure bundling optimizations discard unreferenced symbols.
- **R-LUCIDE-003** SHOULD: Icon elements SHOULD receive shared utility styling for size and color to preserve token alignment across components.

### Verify

```bash
# Discover and run the project static analysis and linting scripts
# Discover and execute the project build and type-checking scripts
```

**Accept when:**
- All UI components requiring visual indicators import exclusively from lucide-react.
- Static verification and type inspection suites pass with zero unresolved symbol references or dependency errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated dependency analysis, static lint rules, and peer review verification apply.
</enforcement>