---
name: guided-gui-design
description: Use when designing or reviewing a user-facing GUI and the goal is to reduce first-use cognitive load, clarify the primary path, improve discoverability and feedback, remove internal jargon, prevent common errors, and ground decisions in established HCI and usability principles.
---

# Guided GUI design

Use this skill for user-facing screens where the product itself
must teach the user how to use it.

This skill is grounded in established HCI and usability principles
rather than multimedia-learning rules for tutorials. Use it to
redesign the screen around the user's task, the visible system
state, the next action, and the result.

Read [references/hci-usability-foundations.md](references/hci-usability-foundations.md)
when you need the underlying rationale, source names, or a deeper
review lens.

## Core outcome

Design the screen so a first-time user can understand:

1. What this screen helps them achieve.
2. What they should do first.
3. What state they are acting on now.
4. What result matters most.
5. How to recover if they make a mistake.

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

The agent MUST state the goal in plain task language.

- Good: `Capture, annotate, and save the correct tutorial shot`
- Bad: `Manage shot manifests`, `Edit rendering metadata`

The UI MUST be organized around the user's job, not the internal
data model.

### 2. Define the primary path and state model

Before changing layout, the agent MUST state:

1. The primary user path.
2. The minimum setup required.
3. The important system states.
4. The result the user should evaluate.

Example:

1. Select the target item.
2. Make the change.
3. Review the result.
4. Save or retry.

If the screen mixes multiple jobs, the agent MUST separate them or
subordinate the secondary jobs.

### 3. Reduce first-use cognitive load

The agent MUST reduce what the user must think about at the start:

- Show only the next meaningful decision first.
- Keep secondary controls visually quieter.
- Delay advanced detail until the user has context.
- Prefer one clear next action over many equal-weight choices.
- The agent MUST NOT require users to remember hidden rules from
  previous screens.
- Prefer self-revealing UI over persistent instructional prose.
- Use layout, ordering, grouping, labels, and feedback to teach
  the flow first.
- The agent MUST add explanatory prose only when the next action
  cannot be inferred reliably from the UI itself.

The agent MUST NOT split into beginner/expert modes unless the
user explicitly asks for that boundary.

### 4. Remove internal wording and recall burden

The agent MUST replace irrelevant implementation language and
site-only jargon with ordinary task language.

- Good: `Choose the shot to edit`
- Bad: `Select output artifact`, `Bind manifest target`

Recognition is better than recall. The agent MUST prefer visible
labels, examples, current selection indicators, and inline
explanations over requiring the user to remember prior context.

If a precise operator or domain term is genuinely part of the
user's real work, the agent MUST keep it. The agent MUST remove
only wording that reflects the implementation model rather than
the user's task. If a domain term still needs support, the agent
MUST explain it inline without breaking the flow.

### 5. Make state and next action obvious

At every stage, the user MUST be able to answer:

- Where am I.
- What am I acting on right now.
- What should I do now.
- What happens if I do it.
- What changed after I did it.

If the screen cannot answer those questions quickly, the agent
MUST simplify the structure or wording.

In interactive flows such as drag-and-drop, selection, move,
compare, and annotation, the agent MUST always make the current
selection, source item, target, and result visually distinct. The
user MUST be able to answer `what am I acting on right now?` at a
glance, without reading a status message or tooltip.

### 6. Lower interaction cost

Primary actions MUST be easy to notice, reach, and hit:

- Make common targets large enough.
- Keep the main action near the working area or the decision it
  commits.
- Avoid forcing long pointer travel between tightly coupled
  controls.
- Reduce the number of equally prominent choices shown at once.

If users must stop and compare many options before they can start,
the agent MUST simplify the choice set or stage it.

### 7. Prevent errors and support recovery

The agent MUST design for mistake resistance, not just happy-path
completion:

- Make destructive or irreversible actions explicit.
- Prevent invalid states before they happen when possible.
- Show validation near the control that caused it.
- Make undo, retry, reset, and cancellation easy to find.
- Keep draft work safe when recreation would be costly.

The agent MUST NOT hide recovery behind documentation when the UI
can present it directly.

### 8. Show the most decision-relevant result first

Once inputs are in place, the agent MUST put the main result or
next checkpoint first:

- Identify the one result the user should decide from.
- Place that above supporting diagnostics.
- Move explanation, assumptions, and metadata later or collapse
  them.

### 9. Verify the screen like a first-time user

The agent MUST verify that a user can succeed by following the
page in order.

Minimum review questions:

1. Can I use this by reading from the top or following the visual
   path?
2. Do I know what this screen is for within a few seconds?
3. Do I know what I am acting on right now?
4. Do I know which action to take next?
5. Do I see what changed after I acted?
6. Can I recover if I make a mistake?
7. Do I see internal jargon that users would not understand?
8. Does the primary UI avoid horizontal scrolling at normal
   viewport sizes?
9. Are there any overflow, clipping, or unexpected text-wrapping
   issues?
10. Can the primary flow be completed with keyboard and visible
    focus cues?

For any major GUI change, the agent MUST take screenshots and do a
visual review before treating the work as complete. The agent MUST
spot-check normal and reduced widths.

## Common fixes

- Move setup above results.
- Rename sections to task language.
- Replace empty states with a clear next action.
- Move validation next to the offending control.
- Collapse metadata and advanced detail until needed.
- Add persistent selection or source/target highlighting.
- Put save, apply, or confirm actions next to the artifact they
  commit.
- Add undo, reset, or retry where the user currently gets stuck.

## Deliverable expectation

When using this skill, the agent MUST describe changes in user
terms:

- What task is easier now.
- What the user now sees first.
- What the user now does next.
- What confusion or error risk was removed.
- What evidence supports the claim.

The agent MUST NOT center the report on internal component names
unless asked.
