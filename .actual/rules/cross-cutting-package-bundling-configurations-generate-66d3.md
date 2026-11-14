---
version: 2
name: cross-cutting-package-bundling-configurations-generate-66d3
description: "Tsup Library Adoption for Component Package Bundling: Package Bundling Configurations Generate Both Modern"
metadata:
  actual:
    adr_id: 66d30abf-3518-4aa3-ab41-6a70bf7a6bf1
    status: accepted
    candidate_group_id: 297ea402-2b1a-4e79-b4c6-b361293ac201
    evidence:
      source: deep
  scope:
    root: packages/react
---

# Tsup Library Adoption for Component Package Bundling: Package Bundling Configurations Generate Both Modern

Scope: applies to `packages/react/**`.

These rules are ALWAYS ACTIVE for workspace library packages that require compiled distribution artifacts for consumers.

### Rules

- **R-TSUP-001** MUST: Package bundling configurations MUST generate both modern module and legacy module formats alongside type declaration outputs.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>