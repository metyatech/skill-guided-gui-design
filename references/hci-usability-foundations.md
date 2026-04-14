# HCI and usability foundations

Use this file when you need the theory behind the
`guided-gui-design` skill or when you want to justify a GUI
design recommendation with a named principle.

## How to use these foundations

- Use them as diagnostic lenses, not rigid laws.
- Prefer multiple converging signals over one slogan.
- When principles conflict, prioritize user task success, error
  prevention, and recoverability.
- Cite the named principle when it materially changes the
  recommendation; do not name-drop for its own sake.

## 1. Design philosophy

### Task-centered and human-centered design

The GUI should match the user's real task and context of use,
not the system's internal model.

Implications:

- Start by naming the user's job in plain language.
- Group controls by the task sequence, not by implementation
  layers.
- Keep secondary or administrative detail behind the primary
  flow.
- Validate designs against real users in a realistic context,
  not only against the team's internal mental model.

Suggested sources:

- ISO 9241-210, _Human-centred design for interactive systems_
- Don Norman, _The Design of Everyday Things_

### Mental model vs conceptual model vs system image

Users build a **mental model** of how a system works from the
**system image** the UI presents. The designer's
**conceptual model** must be communicated through that image,
or the user's mental model will diverge and produce errors.

Implications:

- Make the conceptual model explicit through visible structure,
  labels, and feedback.
- When the implementation model is unusual, hide it; do not
  force users to learn it.
- Mismatch between expected and actual behavior is a UI bug,
  not a user bug.

### Gulfs of execution and evaluation

The **gulf of execution** is the gap between user intent and
the actions the system permits. The **gulf of evaluation** is
the gap between system output and the user's understanding of
what happened.

Implications:

- Close the execution gulf with discoverable controls and
  affordances that match user intent.
- Close the evaluation gulf with immediate, interpretable
  feedback.

## 2. Norman's interaction principles

### Affordance and signifier

An **affordance** is a possible action the environment offers.
A **signifier** is a perceivable cue that reveals the
affordance to the user. In digital UIs, almost all useful
affordances must be signaled.

Implications:

- Make possible actions visually obvious; do not rely on
  hover-only discovery for primary actions.
- Buttons, drag handles, resizers, and editable fields must
  look like what they do.

### Mapping

The relationship between control and effect should match user
expectation, often through spatial layout, ordering, or shape.

Implications:

- Place controls near the things they affect.
- Order options the way users think about them, not the way
  the database stores them.

### Constraints

Physical, logical, semantic, and cultural constraints reduce
error by limiting possible actions to valid ones.

Implications:

- Disable actions that are invalid in the current state, with
  a visible reason.
- Use input types and ranges that prevent invalid entry.

### Feedback

Every user action needs a visible, interpretable response.

Implications:

- Give immediate, localized feedback after every state-changing
  action.
- Distinguish progress, success, partial success, and failure
  with more than color.

### Conceptual model and consistency

A coherent conceptual model lets users predict behavior from
prior interactions.

Implications:

- Use the same word for the same concept everywhere.
- Use the same control for the same kind of decision.
- Group related concepts so the structure matches the model.

Suggested source:

- Donald A. Norman, _The Design of Everyday Things_

## 3. Heuristic frameworks

### Nielsen's 10 usability heuristics

1. Visibility of system status.
2. Match between system and the real world.
3. User control and freedom.
4. Consistency and standards.
5. Error prevention.
6. Recognition rather than recall.
7. Flexibility and efficiency of use.
8. Aesthetic and minimalist design.
9. Help users recognize, diagnose, and recover from errors.
10. Help and documentation.

Use as a structured review checklist after a design pass.

Suggested source:

- Jakob Nielsen, _10 Usability Heuristics for User Interface
  Design_

### Shneiderman's 8 golden rules of interface design

1. Strive for consistency.
2. Seek universal usability.
3. Offer informative feedback.
4. Design dialogs to yield closure.
5. Prevent errors.
6. Permit easy reversal of actions.
7. Keep users in control (internal locus of control).
8. Reduce short-term memory load.

Particularly useful for transactional flows where steps must
end with clear closure.

Suggested source:

- Ben Shneiderman, _Designing the User Interface_

## 4. Cognition and memory

### Working memory limits

Working memory is small and decays quickly. Classic estimates
give roughly 7±2 chunks (Miller); more recent work gives
roughly 4±1 (Cowan) for genuinely independent items.

Implications:

- Do not require users to compare more than a handful of
  options at once.
- Do not require users to remember state across screens; show
  it.
- Group related items into chunks (Gestalt + labels) so they
  count as fewer items.

Suggested sources:

- George A. Miller (1956), _The Magical Number Seven, Plus or
  Minus Two_
- Nelson Cowan (2001), _The magical number 4 in short-term
  memory_

### Cognitive load theory

Total cognitive load splits into:

- **Intrinsic** — inherent difficulty of the task.
- **Extraneous** — caused by poor presentation; reducible by
  design.
- **Germane** — effort spent building useful mental models.

Implications:

- Cut extraneous load aggressively (jargon, irrelevant detail,
  dense layout, redundant decisions).
- Preserve germane load that helps the user learn the task.
- Match intrinsic load to user expertise; offer scaffolding for
  novices.

Suggested source:

- John Sweller (1988 onward), cognitive load theory

### Recognition over recall

Recognizing a presented option is much faster and more reliable
than recalling it from memory.

Implications:

- Show available actions, recent items, current selection, and
  valid inputs.
- Avoid command-line-style flows in product GUIs unless the
  audience expects them.

### Progressive disclosure

Reveal complexity as the user needs it, not all at once.

Implications:

- Start with the most common 1–3 actions.
- Move advanced options behind a clear, discoverable expansion.
- The agent MUST NOT split into beginner/expert modes unless
  the user explicitly asks for that boundary; progressive
  disclosure is preferred over mode switching.

## 5. Quantitative interaction laws

### Hick-Hyman law

Decision time grows roughly logarithmically with the number of
equally weighted choices.

Implications:

- Do not present many equal-weight options at the start of a
  flow.
- Use sensible defaults when safe.
- Break large choice sets into stages or categories.

Suggested sources:

- W. E. Hick (1952)
- Ray Hyman (1953)

### Fitts's law

Time to acquire a target depends on its size and distance:
small or far targets are slow.

Implications:

- Make primary controls large enough for the input device.
- Place tightly coupled controls close together.
- Place the primary confirmation action near the artifact it
  commits.
- Treat screen edges and corners as effectively infinite-size
  targets on desktop.

Suggested source:

- Paul Fitts (1954)

### Steering law

Time to navigate through a constrained path (a tunnel)
increases with path length and decreases with width.

Implications:

- Avoid long, narrow hover paths between menu and submenu.
- Widen hit areas along common interaction corridors.

### Power law of practice

Time to perform a repeated task decreases as a power function
of repetition.

Implications:

- Provide shortcuts and bulk actions for frequent users.
- Keep the novice path discoverable; do not block it with the
  expert path.

## 6. Visual perception and structure

### Gestalt principles

People infer structure from grouping cues:

- **Proximity** — items close together are perceived as a
  group.
- **Similarity** — items sharing color, shape, or size are
  perceived as related.
- **Continuity** — items aligned along a line or curve are
  perceived as connected.
- **Closure** — partial shapes are perceived as complete.
- **Common region** — items inside the same boundary are
  perceived as a group.
- **Common fate** — items moving together are perceived as a
  group.
- **Figure/ground** — visual systems separate foreground
  objects from background.

Implications:

- Use spacing and alignment as the primary grouping mechanism;
  use borders sparingly.
- Use stronger visual weight for the current selection and the
  primary action.
- Do not force users to read every label to understand
  structure.

### Visual hierarchy

The eye lands first on the most visually prominent element.
Importance must match prominence.

Implications:

- Rank importance and assign visual weight (size, contrast,
  color, position) accordingly.
- The most important element on the screen should be the most
  visually prominent.
- Avoid competing emphasis; if everything is loud, nothing is.

### Scanning patterns

On content-heavy pages, eyes tend to scan in F or Z patterns.
On form-heavy or app-like screens, scanning follows visual
weight and grouping rather than fixed patterns.

Implications:

- Place the most important content where scanning starts.
- Front-load labels and headings with information-bearing
  words.

### Information scent

Users follow links and labels that smell like the destination
they want.

Implications:

- Make labels concretely describe the destination or result.
- Avoid generic labels (`More`, `Details`, `Click here`) when
  the user is choosing between alternatives.

Suggested source:

- Peter Pirolli, _Information Foraging Theory_

## 7. Errors and safety

### Slips vs mistakes

- A **slip** happens when the user has the right intent but
  executes the wrong action (typo, misclick).
- A **mistake** happens when the user has the wrong intent
  (misunderstanding the system).

Implications:

- Reduce slips with input constraints, larger targets,
  confirmation for destructive actions, and undo.
- Reduce mistakes with clearer conceptual model, better
  feedback, and previews before commit.

Suggested source:

- James Reason, _Human Error_

### Poka-yoke

Design that prevents the error from being possible at all.

Implications:

- Disable invalid options instead of accepting and rejecting
  them.
- Use input types and constraints that fit the data.
- Default to the safe choice; require deliberate action for
  the dangerous one.

### Forgiveness and reversibility

When prevention is not possible, recovery must be cheap.

Implications:

- Provide undo for any reversible action.
- For irreversible actions, require explicit confirmation and
  show what will be lost.
- Preserve draft work across reloads, accidental closes, and
  restarts when recreation would be costly.

## 8. Accessibility and inclusive design

### POUR principles (WCAG)

A GUI is incomplete if users cannot reliably:

- **Perceive** the content (contrast, alternative text, captions,
  non-color cues).
- **Operate** the controls (keyboard, sufficient time, no
  seizure-inducing motion, visible focus).
- **Understand** the information (clear language, predictable
  behavior, input assistance).
- **Robust** — works across assistive technologies and future
  user agents.

### Practical baseline

- Visible keyboard focus on every interactive control.
- Sufficient contrast (WCAG 2.1 AA: 4.5:1 normal text, 3:1
  large text and UI components).
- Accessible name on every interactive control.
- State cues never conveyed by color alone.
- Logical reading and focus order.
- Respect for `prefers-reduced-motion` and
  `prefers-reduced-transparency` when those carry meaning.

### Inclusive design beyond disability

The same techniques help users in degraded conditions: bright
sun, small screens, one hand on a phone, slow networks,
unfamiliar language.

Suggested sources:

- W3C, _Web Content Accessibility Guidelines (WCAG) 2.1 / 2.2_
- Microsoft, _Inclusive Design Toolkit_

## 9. Platform conventions

Users carry expectations from the platforms they already use.
Following platform conventions is the cheapest way to feel
familiar; deviating is expensive and must earn its keep.

- **Material Design** — Google, Android, web. Strong opinions on
  elevation, motion, and component behavior.
- **Apple Human Interface Guidelines (HIG)** — iOS, iPadOS,
  macOS, visionOS. Strong opinions on hierarchy, gestures, and
  system controls.
- **Fluent Design / Windows App SDK** — Windows. Strong opinions
  on layering, acrylic, and command surfaces.

Implications:

- Use the platform's primary-button placement, navigation
  pattern, modal behavior, and form-control styling unless
  there is a clear reason to deviate.
- When building cross-platform, choose one consistent system
  per surface rather than mixing idioms.

## 10. Evaluation methods

Pick the cheapest method that exposes the actual failure mode.

- **Heuristic evaluation** — score against Nielsen 10 or
  Shneiderman 8. Cheap, repeatable, biased toward known
  patterns.
- **Cognitive walkthrough** — step through the primary task
  asking: will the user know to do this, see how, understand
  feedback?
- **First-click / first-use test** — show the screen cold and
  observe where the user looks and clicks first.
- **Think-aloud** — have the user narrate reasoning while using
  the screen.
- **A/B comparison** — compare two designs on a specific
  measurable outcome.
- **Accessibility audit** — automated checker plus manual
  keyboard and screen-reader pass.

## 11. Verification lenses

When reviewing a GUI, gather evidence on:

- first-use success rate
- time to first successful action
- wrong-action rate and recovery success
- clarity of current selection and system state
- overflow, clipping, and reduced-width behavior
- keyboard reachability and visible focus
- contrast and non-color state cues
- behavior at the edges (empty, full, error, slow network,
  offline)

Prefer screenshots, observed task walkthroughs, and concrete
failure cases over abstract claims that the UI is "intuitive."
