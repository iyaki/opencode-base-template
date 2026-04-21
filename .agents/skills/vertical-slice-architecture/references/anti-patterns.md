# Anti-Patterns: Vertical Slice in Practice

Use this sheet to diagnose structural drift quickly.

## Slice Theater

Symptom:
- Folder names are slice-based, but logic still lives in global service/repository layers.

Risk:
- High coupling and unclear ownership.

Fix:
- Move behavior orchestration into each slice.
- Keep shared modules infrastructure-focused, not behavior-focused.

## Cargo-Cult DDD

Symptom:
- Many entities/value objects but little meaningful business language or invariants.

Risk:
- Extra complexity without domain clarity.

Fix:
- Model only where rules are non-trivial.
- Remove abstractions that do not enforce business constraints.

## Adapter Explosion

Symptom:
- Ports/adapters created for stable internals that rarely change.

Risk:
- Boilerplate and indirection tax.

Fix:
- Add ports only for volatile, external, or test-critical boundaries.
- Collapse unnecessary interfaces.

## Hidden Shared Monolith

Symptom:
- `shared/` becomes the largest module and contains feature behavior.

Risk:
- Vertical boundaries collapse over time.

Fix:
- Keep shared code limited to cross-cutting utilities and stable primitives.
- Move feature decisions back to owning slice.

## Cross-Slice Reach-Through

Symptom:
- One slice directly imports another slice internals.

Risk:
- Tight coupling and cascading regressions.

Fix:
- Interact through public contracts/events.
- Extract stable shared primitives only when justified.

## Layer-First Regression

Symptom:
- New work is organized globally by `controllers/services/repos` again.

Risk:
- Recreates the original pain Vertical Slice aimed to solve.

Fix:
- Enforce feature-first placement in reviews.
- Use architecture checks to prevent cross-layer drift.
