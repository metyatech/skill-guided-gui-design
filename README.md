# skill-guided-gui-design

Agent skill for designing user-facing GUIs with low first-use cognitive load.

It focuses on:

- natural top-to-bottom or left-to-right task flow
- setup before action, action before result
- removing internal jargon
- making the next action obvious
- making the screen understandable without external explanation

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
- rewriting UI copy so it teaches itself
- checking whether a screen can be used by reading from the top
- reviewing interactive flows for unclear selection or drag state
- auditing layout for overflow, clipping, or horizontal scrolling issues

## Visual quality checklist

Run these checks before marking any major GUI change complete.

### Layout and overflow

- [ ] No horizontal scrollbar appears in the primary UI at the normal target viewport width
- [ ] No content is clipped or hidden by `overflow: hidden` that the user needs to see
- [ ] Text wraps gracefully — no word or phrase is cut mid-line in a way that changes meaning
- [ ] Check at 80% and 120% browser zoom to catch edge cases

**How to verify:** Take a screenshot at normal size. Then resize the window to roughly two-thirds width and take another. Compare both for clipping, scroll, or wrap regressions.

### Interactive selection state

Interactive flows include: drag-and-drop, item selection, move/copy/reorder, compare mode, multi-select.

- [ ] The currently selected item, the choice list, and the destination of the chosen result are visually and spatially explicit
- [ ] The currently selected item has a distinct visual treatment (border, background, icon, or bold label) that does not rely on colour alone
- [ ] Source and target zones are visually differentiated from each other and from neutral areas
- [ ] The user can identify "what am I acting on right now?" without reading a tooltip or status bar
- [ ] Deselected and empty states look clearly different from selected ones
- [ ] Prefer anchored drawers, callouts, overlays, or similar patterns over detached panels when they improve comprehension

**Example — good:** A list of items where the selected row has a solid left border, a light background fill, and its label is bold. Dragging it highlights the drop zone with a dashed border.

**Example — bad:** Selected item has only a colour change from grey to blue, with no shape or weight change. Users with colour-vision deficiency cannot distinguish the state.

### Screenshots and visual review

- Take a screenshot immediately after implementing any major layout change
- Comprehension wins over style: stylistic richness or "game-like" presentation is not success if users cannot immediately tell what is selected, what they are choosing from, and where the change will apply
- Compare against the previous screenshot or design reference before continuing
- If the diff shows unexpected overflow or alignment, fix before moving on
- Do not rely solely on code review to catch layout regressions; visual review is required

## License

MIT
