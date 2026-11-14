---
version: 2
name: cross-cutting-developers-discover-repository-dependenc-bb2b
description: "Client Component Directive (\"use client\") in @openstatus/ui Component Library: Developers Discover Repository Dependency Manifest Inspect"
metadata:
  actual:
    adr_id: bb2b3b54-7149-401a-8329-fdb6de47b115
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

# Client Component Directive ("use client") in @openstatus/ui Component Library: Developers Discover Repository Dependency Manifest Inspect

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** MUST: Developers MUST discover the repository dependency manifest and inspect the authoritative lock artifact to verify the exact resolved version of the rendering framework and UI primitive libraries before introducing or updating client component boundaries.
- **R-UI-002** MUST: Place the client component directive as the very first statement in the module, preceding all import statements and comments.
- **R-UI-003** MUST: When composing composite blocks from multiple UI primitives, ensure that any child component requiring DOM event handlers or state hooks is enclosed within a client boundary.

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