# Adoption Playbook: Incremental Vertical Slice

Adopt gradually. Avoid big-bang rewrites.

## Phase 1: Start with New Work

- Implement all new features as slice.
- Keep legacy layers intact until touched.
- Document one canonical slice template and naming rules.

Exit criteria:
- Team can deliver one complete feature entirely inside one slice.

## Phase 2: Migrate High-Change Legacy Flows

- Pick one frequently changed legacy workflow.
- Introduce a new slice that handles the same behavior.
- Route traffic to the new slice and deprecate old path.

Exit criteria:
- At least one legacy flow migrated without downtime or behavior regressions.

## Phase 3: Add Guardrails

- Add review checklist for slice ownership and dependency direction.
- Add architecture checks where possible.
- Track rejected PR reasons to refine guardrails.

Exit criteria:
- Repeated review failures decline over time.

## Phase 4: Calibrate Pattern Depth

- Add DDD depth only where rule complexity demands it.
- Add Hexagonal seams only around volatile external integrations.
- Keep stable CRUD paths intentionally simple.

Exit criteria:
- Architecture complexity tracks domain complexity, not team preference.

## Governance Prompts for PR Review

- What use case owns this behavior?
- Does this change leak behavior into global shared modules?
- Are dependency directions consistent with slice boundaries?
- Is this abstraction justified by volatility or reuse evidence?

## Metrics to Watch

- Lead time per feature
- Regression rate per release
- Number of files touched per feature
- Cross-slice dependency count

Use these metrics to adjust depth and guardrails, not to enforce dogma.
