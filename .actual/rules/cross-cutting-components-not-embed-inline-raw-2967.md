---
version: 2
name: cross-cutting-components-not-embed-inline-raw-2967
description: "lucide-react Icon Library Adoption: Components Not Embed Inline Raw Svg"
metadata:
  actual:
    adr_id: 29679458-29e8-42fc-9f2b-cb250696b15f
    status: accepted
    candidate_group_id: f22ce03f-373f-4801-9d55-fb8cd1a6ebfb
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
---

# lucide-react Icon Library Adoption: Components Not Embed Inline Raw Svg

Scope: applies to `packages/ui/src/components/**`.

These rules are ALWAYS ACTIVE for all UI component primitives and composite status blocks within the design system package requiring vector iconography.

### Rules

- **R-ICON-001** MUST_NOT: UI components MUST NOT embed inline raw SVG paths or import disparate third-party icon libraries for standard component affordances.
- **R-ICON-002** MANDATORY: Consumers MUST import required icon symbols directly via named imports from lucide-react to ensure bundling optimizations discard unreferenced symbols.
- **R-ICON-003** SHOULD: Icon elements SHOULD receive shared utility styling for size and color to preserve token alignment across components.
- **R-ICON-004** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact, look up official documentation for that exact version, confirm API existence, and re-verify per dependency at point of use.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify no unauthorized icon dependencies or inline SVG elements are introduced in UI components.
# Discover and execute the project build and type-checking scripts to confirm that all imported icon identifiers resolve successfully against the locked dependency version.
```

**Accept when:**
- All UI components requiring visual indicators import exclusively from lucide-react.
- Static verification and type inspection suites pass with zero unresolved symbol references or dependency errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>