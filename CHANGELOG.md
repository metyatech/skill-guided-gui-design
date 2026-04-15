# Changelog

All notable changes to this project are documented here.

## [0.3.0] - 2026-04-15

### Changed

- Expanded theory lens table from 8 to 15 lenses: added human-centered design (ISO 9241-210), Norman's gulfs of execution and evaluation, Shneiderman's 8 golden rules, Steering law, visual hierarchy and information scent (Pirolli), Reason's slip/mistake model with Poka-yoke, and platform conventions (Material, HIG, Fluent)
- Restructured workflow from 9 to 11 steps with dedicated sections for deliberate visual structure (Gestalt, hierarchy) and accessibility as a baseline rather than a finishing pass
- Added lightweight evaluation methods section covering heuristic evaluation, cognitive walkthrough, first-click test, and think-aloud
- Added review question 11 (grayscale state-cue check) and question 12 (platform conventions check)
- Added accessibility baseline section to the README visual quality checklist
- Clarified the skill's relationship to the gui-standards rule module: gui-standards states non-negotiable obligations; this skill provides theory, lenses, workflow, and judgement guidance
- Reorganized the references document into 11 numbered sections with named source anchors for each principle

## [0.2.1] - 2026-04-09

### Changed

- Clarified that self-revealing UI should be preferred over persistent instructional prose when the flow can be inferred from structure, order, labels, and feedback
- Clarified that precise operator or domain vocabulary may remain when it matches the user's real work, while irrelevant implementation jargon should still be removed

## [0.2.0] - 2026-04-09

### Changed

- Reframed the skill around established HCI and usability foundations instead of informal GUI heuristics alone
- Added explicit design lenses for Norman, Nielsen heuristics, cognitive load, Hick-Hyman, Fitts, Gestalt, and accessibility
- Expanded the workflow to cover state visibility, interaction cost, error prevention, and recovery
- Added a reference file with theory-to-design mappings and source anchors for deeper justification
- Updated the README review checklist to cover feedback visibility, recovery, and evidence-based GUI review

## [0.1.3] - 2026-03-09

### Changed

- Added guidance for visually separating the current selection, source item, and target in interactive GUI flows
- Added checks for horizontal scrolling, clipping, and awkward text wrapping in primary UI
- Added screenshot-based visual review guidance for major GUI changes

## [0.1.2] - 2026-03-09

### Fixed

- Stopped running CodeQL on this docs-only skill repository when no JavaScript/TypeScript source files are present

## [0.1.1] - 2026-03-09

### Fixed

- Excluded generated `AGENTS.md` and `CLAUDE.md` files from formatting and markdown lint checks so CI validates the source content instead of generated artifacts

## [0.1.0] - 2026-03-09

### Added

- Initial `guided-gui-design` skill
- Core workflow for top-to-bottom GUI flow, jargon reduction, and first-use guidance
