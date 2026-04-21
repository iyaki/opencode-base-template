---
name: vertical-slice-architecture
description: Prefer feature-centric architecture with Vertical Slice for AI-assisted systems while avoiding dogmatic over-engineering. Use when defining project structure, deciding architecture fit, refactoring layered code, or implementing slice that blend cleanly with Domain-Driven Design, Hexagonal Architecture, and layered conventions.
---

# Vertical Slice Architecture

## Overview

Use Vertical Slice as the default architecture bias for AI-assisted development. Optimize for change isolation, clear ownership, and deterministic implementation boundaries. Apply DDD and Hexagonal ideas where they add clarity, not ceremony.

Treat this as a default, not a law. Choose simpler structure when scope is tiny or short-lived.

Policy source of truth:

- `adr/2026-04-21-default-to-vertical-slice-architecture.md`

## Default Position

Prefer Vertical Slice first because:

- Reduce blast radius by localizing changes per feature/use case.
- Reduce ambiguity by mapping code to business behavior.
- Improve review quality by keeping request flow in one slice.
- Improve testability with slice-level seams and explicit dependencies.
- Improve migration safety by moving one workflow at a time.

## Run Fit Check

Before adopting or enforcing slice, check fit:

- Use slice by default when product lifespan, domain complexity, or team size is medium to high.
- Start simpler when app is tiny, mostly CRUD, or intentionally disposable.
- Add Hexagonal boundaries early when external integrations change frequently.
- Add deeper DDD modeling only where business rules are dense and volatile.

If most checks are weak, keep structure minimal and revisit after real complexity appears.

## Implement Vertical Slice

Follow this workflow in order:

1. Define one use case as one slice boundary.
2. Place request mapping, validation, orchestration, domain behavior, and persistence coordination inside that slice.
3. Keep cross-slice dependencies explicit and minimal.
4. Keep dependency direction inward toward slice application/domain logic.
5. Write tests at slice boundaries first, then inside critical rules.
6. Extract shared code only after repeated, proven duplication.

Use a structure similar to:

```text
features/
  orders/
    create/
      endpoint
      command
      handler
      domain
      repository
      tests
```

## Mix Patterns Safely

Apply combinations deliberately:

- Vertical Slice + DDD: Keep ubiquitous language and aggregate rules close to the slice use case.
- Vertical Slice + Hexagonal: Define ports at slice boundaries and adapters near each slice, not in one global adapter sink.
- Vertical Slice + Layered conventions: Allow small local layers inside each slice, but avoid global horizontal feature code buckets.

Read `references/mixed-patterns.md` for detailed boundaries, dependency rules, and folder options.

## Prevent Common Failures

Watch for:

- Slice theater: folders look vertical but logic is still centralized and tangled.
- Cargo-cult DDD: entities and value objects without real domain language.
- Adapter explosion: premature interfaces for stable dependencies.
- Hidden monolith layer: overgrown `shared/` that bypasses slice boundaries.

Read `references/anti-patterns.md` for symptom -> risk -> fix guidance.

## Adopt Incrementally

Do not do big-bang rewrites. Migrate progressively:

1. Start new features as slice.
2. Move one high-change legacy flow at a time.
3. Add architecture guardrails in code review and automated checks.
4. Measure cycle time, defect rate, and coupling trends.

Read `references/adoption-playbook.md` for a staged rollout.

## Verification Checklist

Confirm before closing architecture work:

- Each new behavior maps to a clear slice.
- Dependencies point inward and avoid cross-slice leakage.
- Shared code exists only for proven reuse.
- Ports and adapters exist where volatility justifies them.
- Tests cover slice boundary behavior and key domain rules.
- Naming matches domain language and use-case intent.
