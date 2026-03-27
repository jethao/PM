---
name: pm-write-design-spec
description: Create or revise a Figma-ready design specification for the AirHealth workflow. Use when Codex needs to translate PM/PRD/PRD.md plus PM/Designs/feature.md into a structured design spec in PM/Designs/design-spec.md without changing approved product scope.
---

# pm-write-design-spec

Write or revise the design specification in `PM/Designs/design-spec.md`.

## Use These Inputs

- Primary source of truth: `PM/PRD/PRD.md`
- Secondary reference: `PM/Designs/feature.md`

If the PRD and feature file disagree on product behavior or scope, prefer the PRD unless the user explicitly says otherwise.

If `PM/Designs/feature.md` is missing, continue with the PRD and note that the extra design reference was unavailable.

## Design For Handoff

Produce a design package that another designer could implement in Figma with minimal ambiguity.

Do not introduce new core product requirements silently. If the PRD leaves a gap that affects design, call it out as an ambiguity instead of inventing logic.

Account for the full connected experience when relevant:

- device interactions
- app and account interactions
- onboarding, pairing, and setup
- states and transitions
- error handling and recovery
- physical and digital feedback
- hardware and connectivity constraints

## Required Structure

Keep or add these sections when applicable:

1. Overview
2. Inputs and Alignment
3. Experience Architecture
4. User Flows
5. Screen and Interaction Specification
6. States and Conditions
7. Edge Cases and Failure Handling
8. Feature Specifications
9. Acceptance Criteria
10. Success Metrics
11. Open Design Questions

## What Each Flow Must Define

For each major flow or feature, specify:

- purpose and user goal
- trigger or entry point
- preconditions
- ordered steps
- expected system responses
- alternate and recovery paths
- hardware and software touchpoints
- state-dependent behavior
- acceptance criteria
- success metrics

## Writing Rules

- Keep the design aligned with the approved PRD.
- Cover happy paths and non-happy paths.
- Define what the user sees, what the system is doing, and what the user can do next in meaningful states.
- Be explicit about dependencies on device, firmware, connectivity, entitlement, permissions, or account state.
- Stay Figma-ready, but do not require actual Figma files.

## Self-Check Before Finishing

Confirm all of the following:

- every major flow maps back to a PRD requirement
- critical edge cases and recovery paths are covered
- hardware touchpoints are not ignored
- accessibility implications are called out where needed
- any unresolved PRD ambiguity is surfaced explicitly
