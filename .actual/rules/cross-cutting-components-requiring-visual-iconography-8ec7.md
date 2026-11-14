---
version: 2
name: cross-cutting-components-requiring-visual-iconography-8ec7
description: "lucide-react Icon Library Adoption: Components Requiring Visual Iconography Import Render"
metadata:
  actual:
    adr_id: 8ec7bae1-b20f-4534-a83a-054cc1f9d9f7
    status: accepted
    candidate_group_id: f22ce03f-373f-4801-9d55-fb8cd1a6ebfb
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
---

# lucide-react Icon Library Adoption: Components Requiring Visual Iconography Import Render

Scope: applies to `packages/ui/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LUCIDE-001** MUST: All UI components requiring visual iconography MUST import and render icon components exclusively from the lucide-react library.
- **R-LUCIDE-002** MANDATORY: Consumers MUST import required icon symbols directly via named imports from lucide-react to ensure bundling optimizations discard unreferenced symbols.
- **R-LUCIDE-003** SHOULD: Icon elements SHOULD receive shared utility styling for size and color to preserve token alignment across components.
- **R-LUCIDE-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest in repo. (2) Identify build tool. (3) Inspect lock/resolution artifact for exact version. (4) Look up official docs/changelog for that exact version. (5) Confirm every API/class/function exists in that version. (6) Re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify no unauthorized icon dependencies or inline SVG elements are introduced in UI components.
# Discover and execute the project build and type-checking scripts to confirm that all imported icon identifiers resolve successfully against the locked dependency version.
```

**Accept when:**
- All UI components requiring visual indicators import exclusively from lucide-react.
- Static verification and type inspection suites pass with zero unresolved symbol references or dependency errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated dependency analysis, static lint rules, and peer review verification for all changes adding or updating UI component primitives.
</enforcement>