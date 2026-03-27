---
name: pm-review-package
description: Review the AirHealth PRD or the full PRD-plus-design package for clarity, completeness, feasibility, and revision integrity. Use when Codex needs to audit PM/PRD/PRD.md, cross-check PM/Designs/design-spec.md, and produce actionable findings or update PM/PRD/reviews.md.
---

# pm-review-package

Review the current product documentation rigorously.

## Use These Inputs

- Primary review target: `PM/PRD/PRD.md`
- Package review target, when present: `PM/Designs/design-spec.md`
- Review log, when present: `PM/PRD/reviews.md`
- Feature reference, when useful for source-of-truth checks: `PM/Designs/feature.md`

## Review Goals

Identify anything that would block design, engineering, testing, or implementation handoff.

Review for:

- problem clarity
- scope and non-goals
- user and use-case coverage
- feature behavior and triggers
- hardware and software requirement coverage
- end-to-end execution readiness
- system states, edge cases, and failure handling
- risks, dependencies, and open questions
- internal consistency
- feasibility and realism
- revision history integrity

## Findings Standard

Prefer actionable findings over summaries.

When issues exist, describe:

- severity such as blocker, major, or minor
- what is wrong
- why it matters
- the exact revision action needed

If no material issues are found, say so explicitly and still mention residual risks or testing gaps.

## Revision Integrity Checks

Whenever the PRD has changed, verify:

- the Revision History exists
- the latest revision entry matches the current document state
- accepted review-driven changes are logged accurately
- the review record is not stale relative to the PRD version

Flag any mismatch directly.

## When Updating The Review Log

If the task includes persisting the review in `PM/PRD/reviews.md`:

- append a new review revision instead of overwriting older ones
- identify what document version or package version was reviewed
- record the review date, author, summary, and recommendation
- keep the findings concrete enough that the PRD or design spec can be revised directly from the comments

## Self-Check Before Finishing

Confirm all of the following:

- findings are specific and non-generic
- the review covers both hardware and software implications where relevant
- feasibility concerns are called out, not softened away
- revision-history mismatches are checked explicitly
