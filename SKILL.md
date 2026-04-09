---
name: guided-gui-design
description: Use when designing or reviewing a user-facing GUI and the goal is to reduce first-use cognitive load, clarify the primary path, improve discoverability and feedback, remove internal jargon, prevent common errors, and ground decisions in established HCI and usability principles.
---

# Guided GUI design

Use this skill for user-facing screens where the product itself must teach the
user how to use it.

This skill is grounded in established HCI and usability principles rather than
multimedia-learning rules for tutorials. Use it to redesign the screen around
the user's task, the visible system state, the next action, and the result.

Read [references/hci-usability-foundations.md](references/hci-usability-foundations.md)
when you need the underlying rationale, source names, or a deeper review lens.

## Core outcome

Design the screen so a first-time user can understand:

1. what this screen helps them achieve
2. what they should do first
3. what state they are acting on now
4. what result matters most
5. how to recover if they make a mistake

without needing a separate chat explanation.

## Scientific foundations

Use these principles as design lenses, not as rigid laws.

| Lens                                       | Core question                                                                         | Design implication                                                                          |
| ------------------------------------------ | ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Task-centered design                       | Does the UI match the user's real job?                                                | Organize around the task, not the implementation model                                      |
| Norman: discoverability, mapping, feedback | Can the user tell what is possible, what they selected, and what changed?             | Make actions, selection, system state, and consequences visible                             |
| Nielsen heuristics                         | Does the UI show status, use familiar language, prevent errors, and support recovery? | Prefer visibility, consistency, error prevention, recognition over recall, and user control |
| Cognitive load / working memory            | How much must the user hold in mind at once?                                          | Reduce simultaneous decisions, use progressive disclosure, and keep the next step obvious   |
| Hick-Hyman law                             | Are there too many equally weighted choices?                                          | Limit the number of same-level options shown at once                                        |
| Fitts's law                                | Are primary actions easy to reach and hit?                                            | Make common targets large enough and place them near the working area                       |
| Gestalt / visual perception                | Is grouping and hierarchy obvious at a glance?                                        | Use proximity, alignment, contrast, and shared regions to show structure                    |
| Accessibility / inclusive design           | Can more users perceive and operate the UI reliably?                                  | Ensure labels, focus, contrast, keyboard access, and non-color cues                         |

## Default workflow

### 1. Find the real user goal

State the goal in plain task language.

- Good: `Capture, annotate, and save the correct tutorial shot`
- Bad: `Manage shot manifests`, `Edit rendering metadata`

The UI should be organized around the user's job, not the internal data model.

### 2. Define the primary path and state model

Before changing layout, state:

1. the primary user path
2. the minimum setup required
3. the important system states
4. the result the user should evaluate

Example:

1. select the target item
2. make the change
3. review the result
4. save or retry

If the screen mixes multiple jobs, separate them or subordinate the secondary
jobs.

### 3. Reduce first-use cognitive load

Reduce what the user must think about at the start.

- show only the next meaningful decision first
- keep secondary controls visually quieter
- delay advanced detail until the user has context
- prefer one clear next action over many equal-weight choices
- avoid requiring users to remember hidden rules from previous screens

Do not split into beginner/expert modes unless the user explicitly asks for
that boundary.

### 4. Remove internal wording and recall burden

Replace implementation language and site-only jargon with ordinary task
language.

- Good: `Choose the shot to edit`
- Bad: `Select output artifact`, `Bind manifest target`

Recognition is better than recall. Prefer visible labels, examples, current
selection indicators, and inline explanations over requiring the user to
remember prior context.

If a domain term is genuinely necessary, explain it inline without breaking the
flow.

### 5. Make state and next action obvious

At every stage, the user should be able to answer:

- where am I
- what am I acting on right now
- what should I do now
- what happens if I do it
- what changed after I did it

If the screen cannot answer those questions quickly, simplify the structure or
wording.

In interactive flows such as drag-and-drop, selection, move, compare, and
annotation, always make the current selection, source item, target, and result
visually distinct. The user must be able to answer `what am I acting on right
now?` at a glance, without reading a status message or tooltip.

### 6. Lower interaction cost

Primary actions should be easy to notice, reach, and hit.

- make common targets large enough
- keep the main action near the working area or the decision it commits
- avoid forcing long pointer travel between tightly coupled controls
- reduce the number of equally prominent choices shown at once

If users must stop and compare many options before they can start, simplify the
choice set or stage it.

### 7. Prevent errors and support recovery

Design for mistake resistance, not just happy-path completion.

- make destructive or irreversible actions explicit
- prevent invalid states before they happen when possible
- show validation near the control that caused it
- make undo, retry, reset, and cancellation easy to find
- keep draft work safe when recreation would be costly

Do not hide recovery behind documentation when the UI can present it directly.

### 8. Show the most decision-relevant result first

Once inputs are in place, put the main result or next checkpoint first.

- identify the one result the user should decide from
- place that above supporting diagnostics
- move explanation, assumptions, and metadata later or collapse them

### 9. Verify the screen like a first-time user

Check that a user can succeed by following the page in order.

Minimum review questions:

1. Can I use this by reading from the top or following the visual path?
2. Do I know what this screen is for within a few seconds?
3. Do I know what I am acting on right now?
4. Do I know which action to take next?
5. Do I see what changed after I acted?
6. Can I recover if I make a mistake?
7. Do I see internal jargon that users would not understand?
8. Does the primary UI avoid horizontal scrolling at normal viewport sizes?
9. Are there any overflow, clipping, or unexpected text-wrapping issues?
10. Can the primary flow be completed with keyboard and visible focus cues?

For any major GUI change, take screenshots and do a visual review before
treating the work as complete. Spot-check normal and reduced widths.

## Common fixes

- Move setup above results
- Rename sections to task language
- Replace empty states with a clear next action
- Move validation next to the offending control
- Collapse metadata and advanced detail until needed
- Add persistent selection or source/target highlighting
- Put save, apply, or confirm actions next to the artifact they commit
- Add undo, reset, or retry where the user currently gets stuck

## Deliverable expectation

When using this skill, describe changes in user terms:

- what task is easier now
- what the user now sees first
- what the user now does next
- what confusion or error risk was removed
- what evidence supports the claim

Do not center the report on internal component names unless asked.
