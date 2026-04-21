# Mixed Patterns: Vertical Slice + DDD + Hexagonal + Layered

Use this reference when deciding how to combine architecture patterns without creating contradiction.

## Core Rule

Keep feature/use-case ownership vertical. Use DDD, Hexagonal, and layering as local tools inside that vertical boundary.

## Combination Guide

### Vertical Slice + DDD

Use when business invariants are important.

- Keep ubiquitous language in slice names, commands, and domain types.
- Keep aggregate invariants enforced near slice domain behavior.
- Share domain primitives only when multiple slice genuinely use the same meaning.
- Avoid global domain packages that become dumping grounds.

### Vertical Slice + Hexagonal

Use when external dependencies are volatile or costly to test.

- Define ports from slice application/domain needs.
- Implement adapters close to the slice or in a narrow infra module mapped to that slice.
- Keep port interfaces small and behavior-focused.
- Avoid creating ports for stable internals just to satisfy abstraction purity.

### Vertical Slice + Layered Conventions

Use when the team needs familiar internal structure.

- Keep local mini-layers within each slice (for example: endpoint -> app -> domain -> infra).
- Do not centralize all handlers/services/repos across all features.
- Keep ownership by use case first, layer second.

## Dependency Direction

Prefer this direction:

- interface/api edge -> slice orchestration -> slice domain -> slice ports -> adapters/infra

Avoid this direction:

- slice domain importing transport/web framework details
- one slice directly calling another slice internals

## Folder Strategy Options

### Small systems

```text
features/
  users/register/
  orders/create/
shared/
  db/
```

### Medium systems

```text
features/
  orders/
    create/
    cancel/
  users/
    register/
platform/
  messaging/
  persistence/
```

### Large systems

```text
contexts/
  billing/features/... 
  identity/features/...
shared-kernel/
  money/
  time/
```

Use a shared kernel only for stable, high-value domain primitives.

## Decision Triggers

- Introduce DDD depth when business rules are dense and failures are expensive.
- Introduce Hexagonal seams where external dependencies change often.
- Keep layered structure local to slice for readability, not as a global organizing principle.
