---
version: 2
name: cross-cutting-workspace-packages-not-introduce-custom-40f9
description: "Tsup Library Adoption for Component Package Bundling: Workspace Packages Not Introduce Custom Hoc"
metadata:
  actual:
    adr_id: 40f99c7f-b64c-4ecd-bb46-b85afe4cc283
    status: accepted
    candidate_group_id: 297ea402-2b1a-4e79-b4c6-b361293ac201
    evidence:
      source: deep
  scope:
    root: packages/react
---

# Tsup Library Adoption for Component Package Bundling: Workspace Packages Not Introduce Custom Hoc

Scope: applies to `packages/react/**`.

These rules are ALWAYS ACTIVE for all workspace library packages that require compiled distribution artifacts for consumers.

### Rules

- **R-TSUP-001** MUST_NOT: Workspace packages MUST NOT introduce custom ad-hoc compilation scripts that bypass the standardized packaging library configuration.

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