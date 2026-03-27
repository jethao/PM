---
name: pm-package-gate
description: Run the AirHealth product-document workflow as a staged quality gate. Use when Codex needs to coordinate feature definition, PRD authoring, PRD review, design-spec creation, package review, and a final readiness decision across the files in PM/Designs and PM/PRD.
---

# pm-package-gate

Run the PM document workflow as a concrete staged process, not as a role-playing manager.

## Use These Inputs

- Feature definition: `PM/Designs/feature.md`
- PRD: `PM/PRD/PRD.md`
- Review log: `PM/PRD/reviews.md`
- Design spec: `PM/Designs/design-spec.md`

Use these companion skills when helpful:

- `pm-write-prd`
- `pm-review-package`
- `pm-write-design-spec`

## Enforce This Sequence

1. Confirm the feature definition is present and usable.
2. Create or revise the PRD.
3. Review the PRD before any design work starts.
4. Gate the PRD for clarity, completeness, and feasibility.
5. Create or revise the design spec only after the PRD clears that gate.
6. Review the PRD and design spec together as a package.
7. Reconcile contradictions or unresolved risks.
8. Make a final decision: Approved, Revisions Required, or Blocked.

Do not skip the PRD quality gate and do not let design introduce hidden product logic.

## Quality Bar

Do not approve unless all of the following are true:

- the PRD is clear, concrete, and actionable
- the PRD has been reviewed rigorously
- the design spec aligns with the PRD
- the package is feasible to implement
- major risks, dependencies, and tradeoffs are documented
- open questions are resolved or clearly tracked

## Output Format

When asked to issue a gate decision, use this structure:

### Overall Status

State one of: Approved, Revisions Required, or Blocked.

### Executive Assessment

Summarize whether the package is aligned, complete, and feasible.

### Findings By Workstream

- PRD
- Design Spec
- Review Coverage

### Cross-Functional Gaps

Call out contradictions or missing links across feature definition, PRD, design, and review.

### Feasibility Assessment

State the major risks, dependencies, ambiguities, implementation concerns, and confidence level.

### Decision

State the approval decision and the exact conditions required for approval if the package is not ready.

## Self-Check Before Finishing

Confirm all of the following:

- the work was evaluated in the correct sequence
- blocker issues are not hidden inside summary prose
- the decision is based on evidence in the documents, not momentum
- unresolved contradictions remain open only if they are called out explicitly
