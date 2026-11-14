---
version: 2
name: cross-cutting-components-rendering-interactive-informa-b366
description: "lucide-react Icon Library Adoption: Components Rendering Interactive Informational Icons Provide"
metadata:
  actual:
    adr_id: b3666c3c-ff48-457c-8977-d0391ee74dc3
    status: accepted
    candidate_group_id: f22ce03f-373f-4801-9d55-fb8cd1a6ebfb
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
---

# lucide-react Icon Library Adoption: Components Rendering Interactive Informational Icons Provide

Scope: applies to `packages/ui/src/components/**`.

These rules are ALWAYS ACTIVE for all UI component primitives and composite status blocks requiring vector iconography.

### Rules

- **R-ICON-001** SHOULD: Components rendering interactive or informational icons SHOULD provide accessible label props or enclosing screen-reader text when the icon conveys semantic meaning.
- **R-ICON-002** MANDATORY: Consumers MUST import required icon symbols directly via named imports from lucide-react to ensure bundling optimizations discard unreferenced symbols.
- **R-ICON-003** SHOULD: Icon elements SHOULD receive shared utility styling for size and color to preserve token alignment across components.

### Verify

```bash
# Discover and run the project static analysis and linting scripts
# Discover and execute the project build and type-checking scripts
```

**Accept when:**
- All UI components requiring visual indicators import exclusively from lucide-react.
- Static verification and type inspection suites pass with zero unresolved symbol references or dependency errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated dependency analysis, static lint rules in CI pipelines, and peer review.
</enforcement>