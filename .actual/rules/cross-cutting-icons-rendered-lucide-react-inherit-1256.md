---
version: 2
name: cross-cutting-icons-rendered-lucide-react-inherit-1256
description: "lucide-react Icon Library Adoption: Icons Rendered Lucide React Inherit Stroke"
metadata:
  actual:
    adr_id: 125660f2-401d-4241-a82c-015ef52ae554
    status: accepted
    candidate_group_id: f22ce03f-373f-4801-9d55-fb8cd1a6ebfb
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
---

# lucide-react Icon Library Adoption: Icons Rendered Lucide React Inherit Stroke

Scope: applies to `packages/ui/src/components/**`.

These rules are ALWAYS ACTIVE for all UI component primitives and composite status blocks within the design system package requiring vector iconography.

### Rules

- **R-LUCIDE-001** SHOULD: Icons rendered from lucide-react SHOULD inherit stroke, sizing, and color attributes via utility classes or standard component props to maintain consistent design tokens across components.
- **R-LUCIDE-002** MANDATORY: Consumers MUST import required icon symbols directly via named imports from lucide-react to ensure bundling optimizations discard unreferenced symbols.
- **R-LUCIDE-003** MANDATORY: Consumers MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository (DISCOVERY POLICY).
- **R-LUCIDE-004** MANDATORY: Before writing code that uses a versioned library, developers MUST execute the lock-version grounding process: find the dependency manifest, identify the build tool, inspect the repository lock/resolution artifact, look up official documentation for that exact version, confirm every API/class/function exists, and re-run as needed.
- **R-LUCIDE-005** SHOULD: Icon elements SHOULD receive shared utility styling for size and color to preserve token alignment across components.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify no unauthorized icon dependencies or inline SVG elements are introduced in UI components.
# Discover and execute the project build and type-checking scripts to confirm that all imported icon identifiers resolve successfully against the locked dependency version.
```

**Accept when:**
- All UI components requiring visual indicators import exclusively from lucide-react.
- Static verification and type inspection suites pass with zero unresolved symbol references or dependency errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated dependency analysis, static lint rules in CI pipelines, and peer review. Pull requests introducing unapproved icon libraries or raw inline vector markup into UI components are rejected.
</enforcement>