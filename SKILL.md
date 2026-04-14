---
name: guided-gui-design
description: Use when designing or reviewing a user-facing GUI and the goal is to reduce first-use cognitive load, clarify the primary path, improve discoverability and feedback, prevent errors, ensure accessibility, and ground decisions in established HCI and usability theory.
---

# Guided GUI design

Use this skill for user-facing screens where the product itself
must teach the user how to use it.

This skill complements the `gui-standards` rule module:
`gui-standards` states the non-negotiable obligations; this
skill provides the theory, design lenses, workflow, and
judgement guidance behind them.

Read [references/hci-usability-foundations.md](references/hci-usability-foundations.md)
when you need the underlying rationale, named source anchors,
or a deeper review lens.

## Core outcome

Design the screen so a first-time user can understand:

1. What this screen helps them achieve.
2. What they should do first.
3. What state they are acting on now.
4. What result matters most.
5. How to recover if they make a mistake.

without needing a separate chat explanation.

## Theory lenses

Use these as diagnostic lenses, not as rigid laws. When lenses
conflict, prioritize task success, error prevention, and
recoverability.

| Lens                                                                                                       | Core question                                                                     | Design implication                                                              |
| ---------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Task-centered / human-centered design (ISO 9241-210)                                                       | Does the UI match the user's real job and context of use?                         | Organize around the task, not the implementation model                          |
| Norman: gulfs of execution and evaluation                                                                  | Can the user form intent and judge result without translation?                    | Close both gulfs with discoverable controls and visible feedback                |
| Norman: affordance, signifier, mapping, constraint, feedback, conceptual model                             | Can the user tell what is possible, what they selected, what changed?             | Make actions, selection, system state, and consequences visible                 |
| Nielsen 10 heuristics                                                                                      | Does the UI show status, use familiar language, prevent errors, support recovery? | Prefer visibility, consistency, error prevention, recognition, and user control |
| Shneiderman 8 golden rules                                                                                 | Are consistency, shortcuts, feedback, closure, and reversibility supported?       | Provide consistent flows, undo, dialogue closure, and informative feedback      |
| Cognitive load theory / working memory (Miller, Cowan)                                                     | How much must the user hold in mind at once?                                      | Reduce simultaneous decisions, use progressive disclosure, externalize state    |
| Hick-Hyman law                                                                                             | Are there too many equally weighted choices at one decision point?                | Limit same-level options; stage decisions; use sensible defaults                |
| Fitts's law                                                                                                | Are primary actions easy to reach and hit?                                        | Make common targets large enough; place near the working area                   |
| Steering law                                                                                               | Do users have to navigate narrow paths (menus, tunnels)?                          | Avoid long thin hover paths; widen common interaction corridors                 |
| Gestalt principles (proximity, similarity, continuity, closure, common region, common fate, figure-ground) | Is grouping and hierarchy obvious at a glance?                                    | Use spacing, alignment, contrast, enclosure to show structure                   |
| Visual hierarchy and scanning patterns                                                                     | Does the eye land on the most important thing first?                              | Use weight, size, contrast, and position to rank importance                     |
| Information scent (Pirolli)                                                                                | Do labels and links predict what users will find?                                 | Make labels concretely describe the destination or result                       |
| Reason: slips vs mistakes / Poka-yoke                                                                      | Will common errors be slips (action) or mistakes (intent)?                        | Constrain inputs, confirm destructive intent, make recovery cheap               |
| Accessibility (WCAG / POUR)                                                                                | Can more users perceive, operate, understand, and rely on the UI?                 | Labels, focus, contrast, keyboard, non-color cues, reduced-motion respect       |
| Platform conventions (Material, HIG, Fluent)                                                               | Does the UI behave the way users on this platform already expect?                 | Follow platform idioms; deviate only with a clear reason                        |

## Default workflow

### 1. Find the real user goal

State the goal in plain task language.

- Good: `Capture, annotate, and save the correct tutorial shot`
- Bad: `Manage shot manifests`, `Edit rendering metadata`

The UI MUST be organized around the user's job, not the
internal data model.

### 2. Define the primary path and state model

Before changing layout, state:

1. The primary user path.
2. The minimum setup required.
3. The important system states and transitions.
4. The result the user should evaluate.

Example:

1. Select the target item.
2. Make the change.
3. Review the result.
4. Save or retry.

If the screen mixes multiple jobs, separate them or subordinate
the secondary jobs.

### 3. Reduce first-use cognitive load

Reduce what the user must think about at the start:

- Show only the next meaningful decision first (progressive
  disclosure).
- Keep secondary controls visually quieter.
- Delay advanced detail until the user has context.
- Prefer one clear next action over many equal-weight choices
  (Hick-Hyman).
- Externalize required context through visible labels,
  previews, and current-selection indicators rather than
  requiring recall across screens.
- Prefer self-revealing UI over persistent instructional prose.
- Add explanatory prose only when the next action cannot be
  inferred from the UI itself.

### 4. Remove internal wording and recall burden

Replace irrelevant implementation language and site-only jargon
with ordinary task language.

- Good: `Choose the shot to edit`
- Bad: `Select output artifact`, `Bind manifest target`

If a precise operator or domain term is genuinely part of the
user's real work, keep it. Remove only wording that reflects
the implementation model rather than the user's task. If a
domain term still needs support, explain it inline without
breaking the flow.

### 5. Make state and next action obvious

At every stage, the user MUST be able to answer:

- Where am I.
- What am I acting on right now.
- What should I do now.
- What happens if I do it.
- What changed after I did it.

In interactive flows (drag-and-drop, selection, move, compare,
annotation, multi-select), always make the current selection,
source, target, and result visually distinct without relying on
color alone. Pair color with shape, weight, icon, text, or
position so the state survives color-vision differences and
low-contrast displays.

### 6. Lower interaction cost

Primary actions MUST be easy to notice, reach, and hit (Fitts):

- Make common targets large enough for the input device.
- Keep tightly coupled controls close together.
- Place the primary confirmation action near the artifact it
  commits.
- Avoid narrow, deeply nested hover paths (Steering law).
- Reduce the number of equally prominent choices shown at once.

If users must stop and compare many options before they can
start, simplify the choice set or stage it.

### 7. Prevent errors and support recovery

Design for mistake resistance, not just happy-path completion:

- Distinguish slips (right intent, wrong action) from mistakes
  (wrong intent). Constrain inputs to reduce slips; confirm or
  preview to reduce mistakes.
- Make destructive or irreversible actions visually distinct
  and require explicit confirmation.
- Prevent invalid states at the input boundary when possible
  (Poka-yoke).
- Show validation next to the control that caused it, not only
  in a summary region.
- Make undo, retry, reset, and cancellation easy to find and
  reach during the action, not only after.
- Keep draft work safe when recreation would be costly.

### 8. Show the most decision-relevant result first

Once inputs are in place, put the main result or next
checkpoint first:

- Identify the one result the user should decide from.
- Place it above supporting diagnostics.
- Move explanation, assumptions, and metadata later or collapse
  them.

### 9. Apply visual structure deliberately

Use Gestalt principles and visual hierarchy to communicate
structure before users read any text:

- Proximity and common region group related controls.
- Alignment and consistent spacing reduce visual noise.
- Size, weight, and contrast rank importance.
- Enclosure and figure-ground separate the active area from
  background chrome.

Make the most important element on the screen the most visually
prominent.

### 10. Design for accessibility from the start

Accessibility is a baseline, not a finishing pass:

- Every interactive control needs an accessible name and a
  visible focus indicator.
- The primary flow must work with keyboard alone, in a logical
  order.
- Pair color cues with non-color cues.
- Meet at least WCAG 2.1 AA contrast.
- Respect platform reduced-motion and reduced-transparency
  preferences when conveying state with motion or translucency.

### 11. Verify the screen like a first-time user

Verify that a user can succeed by following the page in order.

Minimum review questions:

1. Can I use this by reading from the top or following the
   visual path?
2. Do I know what this screen is for within a few seconds?
3. Do I know what I am acting on right now?
4. Do I know which action to take next?
5. Do I see what changed after I acted?
6. Can I recover if I make a mistake?
7. Do I see internal jargon that users would not understand?
8. Does the primary UI avoid horizontal scrolling at normal
   viewport sizes?
9. Are there any overflow, clipping, or unexpected text-wrapping
   issues at reduced width?
10. Can the primary flow be completed with keyboard and visible
    focus cues?
11. Do all state cues survive a grayscale view (no color-only
    information)?

For any major GUI change, take screenshots and do a visual
review before treating the work as complete. Spot-check normal
and reduced widths.

## Common fixes

- Move setup above results.
- Rename sections to task language.
- Replace empty states with a clear next action and an example.
- Move validation next to the offending control.
- Collapse metadata and advanced detail until needed.
- Add persistent selection or source/target highlighting that
  does not rely on color alone.
- Put save, apply, or confirm actions next to the artifact they
  commit.
- Add undo, reset, or retry where the user currently gets stuck.
- Increase target size for primary actions on touch.
- Add an accessible name or visible label to icon-only controls.

## Lightweight evaluation methods

Pick the cheapest method that exposes the actual failure mode.

- **Heuristic evaluation** — score the screen against Nielsen's
  10 heuristics or Shneiderman's 8 rules; cheap, catches obvious
  issues, biased toward known patterns.
- **Cognitive walkthrough** — for each step in the primary
  task, ask: will the user know to do this, will they see how,
  will they understand the feedback?
- **First-click / first-use test** — show the screen cold and
  observe where the user looks and clicks first.
- **Think-aloud** — have the user narrate their reasoning while
  using the screen; surfaces mismatches between conceptual
  model and UI.

Prefer concrete failure cases and screenshots over abstract
claims that the UI is "intuitive."

## Deliverable expectation

Describe changes in user terms:

- What task is easier now.
- What the user now sees first.
- What the user now does next.
- What confusion or error risk was removed.
- What evidence supports the claim (screenshots, walkthrough
  result, evaluation against a named heuristic).

The agent MUST NOT center the report on internal component
names unless asked.
