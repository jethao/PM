---
name: pm-write-prd
description: Write or revise an executable PRD for the AirHealth product workflow. Use when Codex needs to turn a feature definition in PM/Designs/feature.md into a concrete PRD in PM/PRD/PRD.md, or update that PRD in response to review feedback while keeping revision history accurate.
---

# pm-write-prd

Write or revise the PRD in `PM/PRD/PRD.md`.

## Use These Inputs

- Primary source of truth: `PM/Designs/feature.md`
- Existing PRD, when present: `PM/PRD/PRD.md`
- Review feedback, when revising: `PM/PRD/reviews.md`

If `PM/Designs/feature.md` is missing, stop and ask for it. Do not invent the feature definition.

If the feature file conflicts with prior assumptions or existing PRD text, prefer the feature file and update the PRD accordingly.

## Write For Execution

Write a PRD that is specific enough for design, engineering, firmware, and cross-functional review to execute without major reinterpretation.

Always cover both sides of the connected experience when relevant:

- device hardware behavior
- firmware or embedded behavior
- mobile, desktop, or web surfaces
- cloud, account, sync, and entitlement behavior
- onboarding, pairing, setup, and recovery
- real-world failure and degraded states

Avoid vague language like "easy", "seamless", or "intuitive" unless you define the expected behavior operationally.

## Required Structure

Keep or add these sections when applicable:

1. Revision History
2. Overview
3. Problem Statement
4. Goals and Non-Goals
5. Target Users and Use Cases
6. Feature Definition
7. End-to-End Experience
8. Functional Requirements
9. System Behavior and States
10. Edge Cases and Failure Scenarios
11. UX and Design Implications
12. Technical and Operational Considerations
13. Success Metrics
14. Risks, Assumptions, and Open Questions
15. Scope and Phased Delivery

Merge or rename sections only when the document stays just as clear and execution-ready.

## Revision History Rules

When updating an existing PRD:

- add a new top revision entry instead of overwriting prior history
- include version, date, author, summary of changes, source of change, and affected sections
- reflect accepted reviewer feedback in both the revision history and the body
- make sure the newest revision entry matches the actual edits

## Writing Rules

- Integrate the feature definition into the PRD; do not append it as a detached summary.
- Separate confirmed requirements from assumptions and open questions.
- Make requirements testable where possible.
- Define state transitions, triggers, and fallback behavior.
- Cover failure modes such as pairing failure, disconnects, offline states, partial setup, permission denial, and mismatch states across device, app, account, and firmware.
- Leave design solutioning to the design spec, but give design enough product requirements to work from.

## Self-Check Before Finishing

Confirm all of the following:

- hardware and software responsibilities are both represented
- cross-surface behavior is internally consistent
- success metrics are measurable
- out-of-scope items are explicit
- recovery behavior is defined for important failures
- the revision history is current and accurate
