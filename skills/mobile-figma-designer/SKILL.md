---
name: mobile-figma-designer
description: Create Figma-ready AirHealth mobile design packages from the latest mobile feature design in SW/feature-design. Use when Codex needs to read the newest Mobile_Feature_Design_v*.md file, stay within consumer mobile-app scope, and produce screen inventories, variants, components, and prototype notes that another designer can build directly in Figma.
---

# mobile-figma-designer

Create or revise a Figma-ready mobile design package from the latest AirHealth mobile feature design.

## Resolve The Source Of Truth First

1. Select the latest file matching `SW/feature-design/Mobile_Feature_Design_v*.md`.
   Prefer the highest version suffix, for example:
   `ls SW/feature-design/Mobile_Feature_Design_v*.md | sort -V | tail -n 1`
2. Treat that latest mobile feature design as the primary source of truth for mobile behavior.
3. Only read extra docs when needed:
   - latest `SW/feature-design/Shared_Integration_Appendix_v*.md` for shared contracts
   - `PM/PRD/PRD.md` if the mobile design leaves a product-level ambiguity
4. If the latest mobile design conflicts with a secondary doc, preserve the mobile-design intent and call out the contradiction instead of silently changing scope.

## Scope Boundaries

Stay inside consumer mobile-app scope.

Do not design:

- firmware internals
- factory-tooling UI
- backend admin or cloud-operator surfaces

Internal-only data such as `HW-ID`, detected VOC type, and factory logs may affect compatibility or edge-state handling, but they must not appear in consumer-facing UI unless the source doc explicitly requires it.

## Default Deliverable

Default to creating or revising `SW/feature-design/Mobile_Figma_Design.md` unless the user names a different output path.

The output should be detailed enough that a product designer can build the flows in Figma without re-deriving screen structure, variants, or key transitions.

If direct Figma editing is unavailable, still produce a Figma-ready design spec rather than a loose summary.

## Required Sections

1. Overview
2. Input Documents Used
3. Design Principles And Assumptions
4. Navigation And Information Architecture Impact
5. Screen Inventory
6. Screen Specifications
7. States And Variants
8. Components And Variants
9. Prototype Flows
10. Accessibility And Content Rules
11. Open Questions And Risks

## What Every Screen Spec Must Include

For every screen or frame family, define:

- purpose
- entry points
- preconditions
- layout hierarchy
- key content
- controls and states
- empty, loading, blocked, and error variants
- transitions in and out
- Figma annotation notes

## Screen Inventory Expectations

Enumerate the concrete frames to build in Figma. At minimum, cover the screen families present in the source mobile design, such as:

- onboarding and pairing
- home and feature hub
- goal setup
- guided measurement
- result summary
- history and detail
- support directory and external handoff
- entitlement, temporary-access, and read-only states
- reconnect and recovery flows
- export settings and outcomes

Prefer explicit variant names over burying important states inside prose.

## Component Expectations

Define reusable components and their variants, such as:

- feature card
- action row
- status banner
- measurement step card
- result card
- entitlement banner
- blocking modal
- support resource row
- export destination row

For each component, specify:

- supported states
- key props or inputs
- icon, illustration, or copy dependencies
- platform differences when they materially affect the design

## Prototype Expectations

Document the prototype links another designer should wire in Figma:

- happy path
- interrupted measurement flow
- entitlement block
- reconnect recovery
- external support handoff
- export flow

Name the trigger and target frame for each transition.

## Writing Rules

- Be frame-oriented and concrete.
- Prefer explicit screen and variant names over abstract UX summaries.
- Keep copy guidance high level unless the source docs define exact wording.
- Preserve the source behavior; do not invent new mobile flows to make the design feel more complete.
- Surface ambiguity clearly when the latest mobile feature design leaves a gap.

## Final Self-Check

Confirm all of the following before finishing:

- the latest mobile feature design was used
- every user-visible mobile flow maps to at least one frame or variant
- blocked, loading, failure, and recovery states are covered
- internal-only data stays out of consumer-facing outputs
- the deliverable is detailed enough for direct Figma construction
