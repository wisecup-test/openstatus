---
version: 2
name: cross-cutting-component-modules-shared-package-wrappin-604b
description: "Client Component Directive (\"use client\") in @openstatus/ui Component Library: Component Modules Shared Package Wrapping Third"
metadata:
  actual:
    adr_id: 604b6be7-c9d1-4622-83e2-3f1d44de1804
    status: accepted
    candidate_group_id: 4802f371-80d7-4d9d-91c4-2dfa24bf57b8
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
      - src/hooks/**
---

# Client Component Directive ("use client") in @openstatus/ui Component Library: Component Modules Shared Package Wrapping Third

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for all component modules in the shared UI package wrapping third-party interactive primitives, utilizing client hooks, or depending on browser runtime capabilities.

### Rules

- **R-UI-001** MUST: Component modules in the shared UI package wrapping third-party interactive primitives that rely on browser DOM events, layout measurements, or client state MUST declare the 'use client' boundary at the wrapper module level.
- **R-UI-002** MUST: Place the client component directive as the very first statement in the module, preceding all import statements and comments.
- **R-UI-003** MUST: Ensure that any child component requiring DOM event handlers or state hooks is enclosed within a client boundary when composing composite blocks from multiple UI primitives.

### Verify

```bash
# Discover and run repository lint and check scripts from project manifests
MANIFEST_SCRIPTS=$(find . -maxdepth 2 -name '*.*' -exec grep -l '"lint"' {} + 2>/dev/null | head -n 1)
echo "Discovered lint configuration in: $MANIFEST_SCRIPTS"

# Discover component files importing interactive hooks and verify client boundary declarations
find . -type f -name '*.*' -exec grep -lE 'useState|useEffect|useMemo' {} + | while read -r comp; do head -n 5 "$comp" | grep -q '"use client"' || echo "Verify client boundary in: $comp"; done

# Discover and run repository build validation scripts to confirm rendering model integrity
MANIFEST_BUILD=$(find . -maxdepth 2 -name '*.*' -exec grep -l '"build"' {} + 2>/dev/null | head -n 1)
echo "Discovered build configuration in: $MANIFEST_BUILD"
```

**Accept when:**
- All UI component modules utilizing client-side hooks or browser event listeners declare the client boundary directive as their initial statement.
- Repository build and lint verification scripts complete with zero exit code and no client-server boundary violation errors.
- Component rendering in both server-side and client-side execution environments completes without boundary mismatch warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification. All interactive UI components in the shared package must have the 'use client' directive explicitly set as the first line, and automated validation scripts must pass without errors.
</enforcement>