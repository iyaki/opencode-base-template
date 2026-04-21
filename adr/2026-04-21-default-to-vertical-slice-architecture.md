---
status: accepted
date: 2026-04-21
decision-makers: Repository maintainers
consulted: AI coding agents, contributors
informed: Future contributors and reviewers
---

# Default to Vertical Slice Architecture

## Context

This repository now includes guidance and a dedicated skill for Vertical Slices Architecture, but the default is not yet formalized as repository policy. Without a written decision, contributors and agents may drift between feature-first and layer-first structures, creating inconsistent implementation patterns and review ambiguity.

For AI-assisted development, explicit architecture defaults improve repeatability and reduce cross-feature coupling. We also need clear boundaries for when to combine Vertical Slices with Domain-Driven Design (DDD), Hexagonal Architecture, and layered conventions.

## Decision

Adopt Vertical Slice Architecture as the repo-wide default for new feature work.

Decision details:

- Organize new behavior by use case/feature first, not by global technical layers.
- Allow local mini-layers inside each slice when useful for readability.
- Apply DDD depth where business rules are dense or volatile.
- Apply Hexagonal ports/adapters where external dependency volatility justifies seams.
- Permit exceptions when a slice-first approach is clearly not a fit, but require explicit rationale in the PR description and maintainer approval.

This ADR is the policy source of truth. The operational implementation guidance lives in:

- `.agents/skills/vertical-slice-architecture/SKILL.md`

## Consequences

Positive:

- Clear default for contributors and agents reduces architecture drift.
- Feature ownership and change impact become easier to reason about.
- Reviews can evaluate work against a consistent structural baseline.
- Incremental modernization becomes safer by migrating use case by use case.

Tradeoffs and risks:

- Possible duplication across slices if sharing is extracted too late.
- Possible over-engineering if DDD/Hexagonal patterns are applied indiscriminately.
- Legacy layered areas will remain mixed during migration.

## Non-goals

- Do not require a big-bang rewrite of existing code into slices.
- Do not ban shared utilities or stable shared primitives.
- Do not force deep DDD modeling or Hexagonal seams in simple CRUD flows.
- Do not enforce one exact folder template for every language/template stack.

## Implementation Plan

- Add and maintain a dedicated skill with practical guidance:
  - `.agents/skills/vertical-slice-architecture/SKILL.md`
  - `.agents/skills/vertical-slice-architecture/references/mixed-patterns.md`
  - `.agents/skills/vertical-slice-architecture/references/anti-patterns.md`
  - `.agents/skills/vertical-slice-architecture/references/adoption-playbook.md`
- Use this ADR in architecture reviews as the default policy reference.
- For new feature PRs, expect slice-first organization unless an approved exception is documented.
- When touching legacy flows, prefer incremental alignment toward slice ownership rather than broad refactors.
- Keep ADR and skill terminology aligned (`default`, `exception`, `slice`, `shared`).

## Verification

- [ ] ADR directory and index exist and reference this decision.
- [ ] New feature work follows slice-first organization by default.
- [ ] Exceptions include explicit rationale and maintainer approval in PR descriptions.
- [ ] Skill guidance and ADR policy remain aligned and cross-linked.
- [ ] No expectation of full immediate migration is implied by reviews.

## Revisit Triggers

Revisit this ADR if one or more of the following occurs:

- Repeated, justified exception requests suggest the default is mis-scoped.
- Delivery lead time or defect rates regress due to architecture overhead.
- Cross-slice dependency and shared-module coupling increase significantly.
- A major platform or repository restructuring introduces conflicting constraints.
