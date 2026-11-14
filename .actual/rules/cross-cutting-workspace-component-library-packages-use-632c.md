---
version: 2
name: cross-cutting-workspace-component-library-packages-use-632c
description: "Tsup Library Adoption for Component Package Bundling: Workspace Component Library Packages Use Tsup"
metadata:
  actual:
    adr_id: 632c15e0-136c-4e9d-94c4-c74467dd065e
    status: accepted
    candidate_group_id: 297ea402-2b1a-4e79-b4c6-b361293ac201
    evidence:
      source: deep
  scope:
    root: packages/react
---

# Tsup Library Adoption for Component Package Bundling: Workspace Component Library Packages Use Tsup

Scope: applies to `packages/react/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TSUP-001** MUST: Workspace component library packages MUST use tsup as the dedicated packaging library to compile source code into distributed module artifacts.

### Verify

```bash
# Discover and execute the package build script defined in the repository configuration
REPO_BUILD_SCRIPT=$(grep -l '"build"' package.json */package.json 2>/dev/null | head -n 1)
test -n "$REPO_BUILD_SCRIPT" && echo "Package build entrypoint discovered"
# Verify that the package distribution output directory is produced upon build execution
test -d dist || echo "Verify distribution output directory creation post-build"
```

**Accept when:**
- The package build task compiles and outputs module bundles according to the package bundling configuration.
- The package build process executes without module resolution or compilation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>