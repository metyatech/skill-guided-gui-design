# skill-guided-gui-design

Agent skill for designing and reviewing user-facing GUIs using
established HCI and usability principles.

It focuses on:

- task-centered GUI structure grounded in human-centered design
- low first-use cognitive load
- self-revealing UI that teaches through structure before prose
- clear system state, selection, feedback, and next action
- error prevention and cheap recovery
- accessibility as a baseline, not a finishing pass
- evidence-based visual and interaction review

## Installation

```sh
npx skills add metyatech/skill-guided-gui-design
```

## Usage

### Claude Code

```text
/guided-gui-design
```

### Codex

```text
$guided-gui-design
```

## When to use it

Use this skill when:

- designing or reviewing a user-facing GUI
- simplifying a dense screen
- reducing first-use confusion
- rewriting UI copy so it supports a self-revealing flow
  instead of carrying it
- checking whether a screen can be used by following the
  primary visual path
- reviewing interactive flows for unclear selection,
  source/target, or drag state
- auditing layout for overflow, clipping, or horizontal
  scrolling issues
- auditing a screen for accessibility baseline
- grounding GUI decisions in HCI or usability theory

## Scientific foundations

This skill is grounded in established HCI, cognitive, and
usability theory rather than tutorial or multimedia-learning
rules.

- **Human-centered design (ISO 9241-210)** — design around real
  user tasks and context of use.
- **Norman** — affordances, signifiers, mapping, constraints,
  feedback, conceptual models, gulfs of execution and
  evaluation.
- **Nielsen's 10 heuristics** — system status, consistency,
  error prevention, recognition over recall, recovery.
- **Shneiderman's 8 golden rules** — consistency, feedback,
  dialogue closure, reversibility, user control.
- **Cognitive load theory and working memory** (Miller, Cowan,
  Sweller) — reduce simultaneous decisions and hidden state.
- **Hick-Hyman law** — do not overwhelm users with many
  equal-weight choices.
- **Fitts's law and Steering law** — keep primary targets
  reachable and interaction corridors wide.
- **Gestalt principles** — proximity, similarity, continuity,
  closure, common region, common fate, figure/ground.
- **Visual hierarchy and information scent** (Pirolli) — rank
  prominence to match importance; make labels predict results.
- **Reason's error model and Poka-yoke** — distinguish slips
  from mistakes; prevent errors by construction.
- **Accessibility / POUR (WCAG)** — perceivable, operable,
  understandable, robust.
- **Platform conventions** — Material, Apple HIG, Fluent.

See [references/hci-usability-foundations.md](references/hci-usability-foundations.md)
for the full set of design implications, named source anchors,
and review lenses.

## Visual quality checklist

Run these checks before marking any major GUI change complete.

### Layout and overflow

- [ ] No horizontal scrollbar appears in the primary UI at the
      normal target viewport width
- [ ] No content is clipped or hidden by `overflow: hidden`
      that the user needs to see
- [ ] Text wraps gracefully — no word or phrase is cut
      mid-line in a way that changes meaning
- [ ] Check at 80% and 120% browser zoom to catch edge cases

**How to verify:** Take a screenshot at normal size. Resize
the window to roughly two-thirds width and take another.
Compare for clipping, scroll, or wrap regressions.

### Interactive selection state

Interactive flows include: drag-and-drop, item selection,
move/copy/reorder, compare mode, multi-select.

- [ ] The currently selected item has a distinct visual
      treatment (border, background, icon, or bold label)
      that does not rely on color alone
- [ ] Source and target zones are visually differentiated from
      each other and from neutral areas
- [ ] The user can identify "what am I acting on right now?"
      without reading a tooltip or status bar
- [ ] Deselected and empty states look clearly different from
      selected ones
- [ ] After an action, the changed state is visible without
      relying on a toast alone
- [ ] Recovery actions such as undo, reset, cancel, or retry
      are easy to find when relevant

**Example — good:** A list where the selected row has a solid
left border, a light background fill, and a bold label.
Dragging it highlights the drop zone with a dashed border.

**Example — bad:** Selected item has only a color change from
grey to blue. Users with color-vision deficiency cannot
distinguish the state.

### Accessibility baseline

- [ ] Every interactive control has an accessible name
- [ ] A visible focus indicator appears on every interactive
      control when focused by keyboard
- [ ] The primary flow completes with keyboard alone, in a
      logical order
- [ ] State cues do not rely on color alone (verify by
      viewing in grayscale)
- [ ] Text and UI component contrast meets WCAG 2.1 AA
- [ ] Motion and translucency respect platform
      reduced-motion/reduced-transparency preferences when
      carrying meaning

### Screenshots and visual review

- Take a screenshot immediately after implementing any major
  layout change
- Compare against the previous screenshot or design reference
  before continuing
- If the diff shows unexpected overflow or alignment, fix
  before moving on
- Do not rely solely on code review to catch layout
  regressions; visual review is required

## Review questions

Use these questions during a first-pass GUI review:

1. Do users know what the screen helps them achieve within a
   few seconds?
2. Is the primary path obvious without external explanation?
3. Is the current state or selection visible at a glance?
4. Is the next action obvious?
5. Is the most decision-relevant result shown first?
6. Are invalid actions prevented or clearly constrained?
7. Can users recover easily from common mistakes?
8. Are the main controls large enough and close enough to the
   object they act on?
9. Does the screen still work at reduced width without
   horizontal scrolling in the primary UI?
10. Can the primary flow be completed with keyboard and
    visible focus cues?
11. Do all state cues survive a grayscale view?
12. Does the screen follow the expected platform conventions,
    or is any deviation clearly justified?

## License

MIT
