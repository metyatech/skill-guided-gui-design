# HCI and usability foundations

Use this file when you need the theory behind the `guided-gui-design` skill or
when you want to justify a GUI design recommendation with a named principle.

## How to use these foundations

- Use them as diagnostic lenses, not rigid laws.
- Prefer multiple converging signals over one slogan.
- When principles conflict, prioritize user task success, error prevention, and
  recoverability.

## Task-centered design

The GUI should match the user's real task rather than the system's internal
model.

Implications:

- Start by naming the user's job in plain language.
- Group controls by the task sequence, not by implementation layers.
- Keep secondary or administrative detail behind the primary flow.

## Don Norman: discoverability, feedback, mapping, constraints, conceptual models

Norman's work is a strong base for interactive systems. The core questions are:

- Can users tell what actions are possible?
- Can they tell what they are acting on now?
- Can they tell what changed after acting?
- Is the control-to-effect relationship obvious?

Implications:

- Make possible actions discoverable without extra explanation.
- Show current selection and mode continuously, not only in transient messages.
- Give immediate, localized feedback after user actions.
- Match control placement and labels to the effect users expect.

Suggested source:

- Donald A. Norman, _The Design of Everyday Things_

## Nielsen's usability heuristics

Jakob Nielsen's heuristics remain one of the most practical review frameworks
for GUI design.

Most relevant heuristics for guided GUIs:

- visibility of system status
- match between system and the real world
- user control and freedom
- consistency and standards
- error prevention
- recognition rather than recall
- flexibility and efficiency of use
- help users recognize, diagnose, and recover from errors

Implications:

- Keep current state visible.
- Use familiar task wording instead of implementation jargon.
- Support cancel, undo, retry, and reset.
- Avoid making users remember hidden state across screens.

Suggested source:

- Jakob Nielsen, _10 Usability Heuristics for User Interface Design_

## Cognitive load and working memory

Working memory is limited. GUIs become hard to use when people must compare too
many choices, remember hidden state, or interpret dense screens before acting.

Implications:

- Stage decisions instead of presenting everything at once.
- Put the next step where the user already looks.
- Defer advanced detail until it is needed.
- Externalize important context through labels, previews, and visible state.

Related ideas:

- cognitive load theory
- progressive disclosure
- recognition over recall

## Hick-Hyman law

Decision time tends to increase as the number of choices grows.

Implications:

- Do not present many equal-weight options at the start of a flow.
- Use sensible defaults when safe.
- Break large choice sets into stages or categories.

Suggested sources:

- W. E. Hick (1952)
- Ray Hyman (1953)

## Fitts's law

Target acquisition time depends on target size and distance.

Implications:

- Make primary controls large enough.
- Keep tightly coupled controls close together.
- Avoid putting the primary confirmation action far from the artifact it
  commits.

Suggested source:

- Paul Fitts (1954)

## Gestalt and visual perception

People infer structure from proximity, similarity, continuity, enclosure, and
visual hierarchy.

Implications:

- Use spacing and grouping to show related controls.
- Use stronger visual weight for the current selection and primary action.
- Do not force users to read every label to understand structure.

## Accessibility and inclusive design

A GUI is incomplete if users cannot reliably perceive or operate it.

Practical baseline:

- visible keyboard focus
- sufficient contrast
- labels for controls and fields
- state cues not conveyed by color alone
- logical reading and focus order

Suggested source:

- W3C WCAG and POUR principles

## Verification lenses

When reviewing a GUI, gather evidence on:

- first-use success rate
- time to first successful action
- wrong-action rate
- recovery success after an error
- clarity of current selection and system state
- overflow, clipping, and reduced-width behavior
- keyboard and focus usability

Prefer screenshots, observed task walkthroughs, and concrete failure cases over
abstract claims that the UI is "intuitive."
