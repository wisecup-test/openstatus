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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ICON-001** MUST_NOT: UI components MUST NOT embed inline raw SVG paths or import disparate third-party icon libraries for standard component affordances.
- **R-ICON-002** MANDATORY: Consumers MUST import required icon symbols directly via named imports from lucide-react to ensure bundling optimizations discard unreferenced symbols.
- **R-ICON-003** SHOULD: Icon elements SHOULD receive shared utility styling for size and color to preserve token alignment across components.
- **R-ICON-004** MANDATORY (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ICON-005** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order:
  1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
  2. Identify the build tool from the manifest.
  3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
  4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
  5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
  6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify no unauthorized icon dependencies or inline SVG elements are introduced in UI components.
# Discover and execute the project build and type-checking scripts to confirm that all imported icon identifiers resolve successfully against the locked dependency version.
```

**Accept when:**
- All UI components requiring visual indicators import exclusively from lucide-react.
- Static verification and type inspection suites pass with zero unresolved symbol references or dependency errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing unapproved icon libraries or raw inline vector markup into UI components are rejected pending refactoring to lucide-react.
</enforcement>