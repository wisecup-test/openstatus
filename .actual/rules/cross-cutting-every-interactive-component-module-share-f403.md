---
version: 2
name: cross-cutting-every-interactive-component-module-share-f403
description: "Client Component Directive (\"use client\") in @openstatus/ui Component Library: Every Interactive Component Module Shared Package"
metadata:
  actual:
    adr_id: f403c5b0-44ae-46f3-b673-1c1c3a81fff1
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

# Client Component Directive ("use client") in @openstatus/ui Component Library: Every Interactive Component Module Shared Package

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for all interactive component modules in the shared UI component package.

### Rules

- **R-CLIENT-001** MUST: Every interactive component module in the shared UI component package that uses client-side state hooks, effect hooks, memoization hooks, or browser event listeners MUST declare the 'use client' directive at the top of the file before any imports or executable statements.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>