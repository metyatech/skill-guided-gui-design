<!-- markdownlint-disable MD025 -->
# Tool Rules (compose-agentsmd)

- **Session gate**: before responding to ANY user message, run `compose-agentsmd` from the project root. AGENTS.md contains the rules you operate under; stale rules cause rule violations. If you discover you skipped this step mid-session, stop, run it immediately, re-read the diff, and adjust your behavior before continuing.
- `compose-agentsmd` intentionally regenerates `AGENTS.md`; any resulting `AGENTS.md` diff is expected and must not be treated as an unexpected external change.
- If `compose-agentsmd` is not available, install it via npm: `npm install -g compose-agentsmd`.
- To update shared/global rules, use `compose-agentsmd edit-rules` to locate the writable rules workspace, make changes only in that workspace, then run `compose-agentsmd apply-rules` (do not manually clone or edit the rules source repo outside this workflow).
- If you find an existing clone of the rules source repo elsewhere, do not assume it is the correct rules workspace; always treat `compose-agentsmd edit-rules` output as the source of truth.
- `compose-agentsmd apply-rules` pushes the rules workspace when `source` is GitHub (if the workspace is clean), then regenerates `AGENTS.md` with refreshed rules.
- Do not edit `AGENTS.md` directly; update the source rules and regenerate.
- `tools/tool-rules.md` is the shared rule source for all repositories that use compose-agentsmd.
- Before applying any rule updates, present the planned changes first with an ANSI-colored diff-style preview, ask for explicit approval, then make the edits.
- These tool rules live in tools/tool-rules.md in the compose-agentsmd repository; do not duplicate them in other rule modules.

Source: github:metyatech/agent-rules@HEAD/rules/global/command-execution.md

# Command execution

Platform-aware execution procedures live in the
`command-execution` skill.

## General execution rules

- Prefer repository-standard scripts and commands (from
  `package.json`, `Makefile`, README) over ad hoc invocations.
  The agent MUST NOT add wrappers, redirections, or pipes
  unless the user explicitly asks.
- Before proposing a fix for a reported command failure,
  reproduce the same command (or the closest equivalent) and
  observe the same failure.
- Treat any nonzero exit code as a failure unless the specific
  exit code is explicitly documented AND the agent's code
  explicitly checks for that documented value. The agent MUST
  NOT map unknown nonzero exits to benign states.
- The agent MUST NOT assume agent platform capabilities beyond
  what is available; fail explicitly when a required capability
  is unavailable.
- When expected tools are missing or configuration has changed,
  verify MCP connectivity, fix or report connection failures,
  and do not proceed with work that depends on the missing tool.
- When diagnosing a third-party tool failure, first check the
  latest stable release; if it still reproduces, record the
  verified limitation and use a deterministic workaround.

## Git and identity flows

- Avoid interactive git prompts (pass `--no-edit` or set
  `GIT_EDITOR=true`).
- When no branch is specified, work on the current branch.
  Direct commits to `main`/`master` are permitted in
  user-controlled repositories.
- For federated identity flows (Google, Apple, Microsoft,
  GitHub) where an automation-launched browser is blocked or
  degraded, hand off only the IdP step to a real browser
  session and resume automation after the redirect. The agent
  MUST NOT attempt to bypass provider anti-automation or
  embedded-browser restrictions.

## Privilege elevation

- When elevated privileges are required, use `sudo` directly.
  The agent MUST NOT launch a separate elevated shell such as
  `Start-Process -Verb RunAs`. Fall back to "Run as
  Administrator" only when `sudo` is unavailable.

## Windows and PowerShell environment

- This is a Windows/PowerShell environment. The agent MUST NOT
  invoke Unix-only commands directly; run PowerShell scripts
  via `pwsh` or `powershell -File`.
- In PowerShell, the backslash `\` is a literal character.
  Avoid shadowing PowerShell automatic variables; prefer
  single-quoted strings. Use `;` for sequential command
  chaining; the agent MUST NOT use `&&` or `||` as control-flow
  operators.
- In headless Windows/PowerShell flows, launch every
  non-interactive console child process headlessly. The agent
  MUST NOT use the `&` call operator from a windowless parent
  to spawn such children.
- For destructive PowerShell file operations, verify the final
  absolute target path first, normalize file attributes when
  needed, and prefer explicit PowerShell or .NET deletion APIs
  over alias-driven shell deletion.
- Use explicit `agent-browser` session names on Windows. If the
  default session bind fails, retry with a different name.
  Close all task-owned agent-browser sessions before concluding.

Source: github:metyatech/agent-rules@HEAD/rules/global/engineering-standards.md

# Engineering and design standards

## Tooling and dependencies

- Prefer official, maintained, latest-stable tools and deps.
  Prefer OSS/free-tier services; document paid/proprietary
  tradeoffs.
- Before building, verify whether an existing official or
  maintained system already satisfies the need; build custom
  logic only for verified gaps.

## System design

- Designs MUST be compositional with clean dependency
  direction, shallow control flow, intention-revealing names,
  and centralized change points.
- Keep code, docs, tests, and config DRY. Fix root causes,
  remove obsolete code in the same change set, and repair
  broken tools at the source.
- Failure paths MUST tear down resources, leave no partial
  state, and verify cleanup.

## Runtime and async behavior

- The agent MUST NOT block async APIs or use sync I/O where
  responsiveness is expected.
- Prefer push, event, or signal synchronization; use polling
  only when no authoritative event path exists or requested;
  document why and bound cadence/retries.

## API surfaces

- Prefer native SDKs and stable APIs; isolate unavoidable
  unstable APIs and externalize large strings/templates.
- The agent MUST NOT commit build artifacts; keep artifact dirs
  and `.gitignore` aligned.

## Linters, formatters, and static analysis

- Every repo MUST have one formatter and one linter/analyzer
  per primary language, pinned in manifests or lockfiles.
- The agent MUST NOT disable lint rules globally. Suppressions
  MUST be narrow, justified inline, and time-bounded. Run
  formatters before replace operations.

## CI enforcement

- CI MUST run formatting/lint checks on every pull request and
  treat warnings as errors.

## Dependency and security scanning

- GitHub Actions repos MUST configure Dependabot for all
  applicable ecosystems, including `github-actions`, unless
  no external update surface exists.
- Repos MUST enable dependency scanning, secret scanning, and
  CodeQL.
- Web UI projects MUST enforce visual accessibility checks.

## Environment portability

- The agent MUST NOT introduce machine-specific environments;
  use relative paths and explicit configuration.
- Lifecycle hooks MUST succeed on a clean machine. Use project
  runners and regenerate lockfiles with manifest changes.
- Agent-owned temp files MUST live under the OS temp dir unless
  explicitly approved otherwise.

## Tool integration

- Design tools/services for agent compatibility via standard
  interfaces. CLI: `cli-design`.
- Every tracked edit in a user-controlled repo MUST go through
  `mwt create`; the agent MUST NOT edit seed checkouts directly
  and MUST NOT commit to seed branches, regardless of whether
  `mwt` is already initialised in the repo.
- If the repo is not yet `mwt`-initialised, the agent MUST run
  `mwt init` in the same session before the first tracked edit.
  `mwt init` MUST track `.mwt/config.toml`, commit it before
  any tracked work, leave no untracked `.mwt/` residue, and
  keep the seed clean.
- If `mwt init` or `mwt create` cannot complete safely or
  deterministically, the agent MUST stop tracked edits and
  report the blocker rather than fall back to direct seed
  editing.
- Run `mwt deliver` before completion in `mwt` repos.
- After `mwt deliver`, run `mwt prune --merged --with-branches`
  for owned worktrees unless asked to keep them.
- In `mwt` repos, do not report completion while delivered
  worktrees remain.

## Post-change deployment verification

- Deployment procedures: `post-deploy`. After code changes,
  determine whether deployment beyond commit/push is needed.
- For globally linked packages, rebuild and verify the global
  binary before reporting completion. For running services,
  daemons, or scheduled tasks, rebuild, restart, and verify
  the running instance. Before concluding, tear down temp
  resources and leave no residue.

Source: github:metyatech/agent-rules@HEAD/rules/global/gui-standards.md

# GUI design and verification

Design principles, theory, and procedural detail live in the
`guided-gui-design` skill. Test design and runtime verification
procedures live in the `quality-workflow` skill.

This module states the non-negotiable obligations. The judgement
guidance and HCI foundations behind them live in
`guided-gui-design`.

## Task and structure

- Design GUIs around the user's real workflow moment, next
  action, and result, not around the system's internal data
  model or implementation layers.
- The agent MUST organize controls by the user's task sequence.
  Group secondary or administrative detail behind the primary
  flow.
- For human/AI systems, keep operator surfaces, backend
  capabilities, and product surfaces distinct. The agent MUST
  NOT expose operator-only or backend-only paths as product GUI
  controls unless the requester explicitly asks.

## State, selection, and feedback

- At every stage the user MUST be able to identify: where they
  are, what they are acting on, what the next action is, what
  will happen if they take it, and what changed after they took
  it.
- In interactive selection flows (drag-and-drop, item
  selection, move/copy/reorder, compare mode, multi-select),
  the agent MUST make the current selection, source, target,
  and result visually distinct without requiring the user to
  read a tooltip, toast, or status bar.
- The agent MUST give immediate, localized feedback after every
  user action that changes state. Transient messages alone MUST
  NOT be the only indication that state changed.

## Cognitive load

- Optimize GUIs for first-use clarity: a new user MUST be able
  to identify the primary path and the next action without
  prior training, using ordinary task language.
- The agent MUST NOT use persistent explanatory prose to
  compensate for an unclear UI. Use layout, ordering, grouping,
  labels, and feedback to teach the flow first; add prose only
  when the next action cannot be inferred from the UI itself.
- The agent MUST replace implementation jargon and site-only
  internal terms with ordinary task language. Keep precise
  operator or domain terms only when they are part of the
  user's real work.
- The agent MUST NOT require users to remember hidden state
  from previous screens. Externalize required context through
  visible labels, previews, and current-selection indicators
  (recognition over recall).
- The agent MUST NOT split a flow into beginner/expert modes
  unless the user explicitly asks for that boundary.

## Conventions and common controls

- Follow established expectations for common controls (info
  icons, disclosure toggles, close buttons, tabs, row
  selection, primary-button placement, destructive-action
  styling). Deviate only when the UX gain clearly outweighs
  the surprise cost, and document the deviation.
- The agent MUST NOT introduce horizontal scrolling in primary
  UI without explicit justification.
- The agent MUST place the primary confirmation action near the
  artifact it commits, not far from the working area.

## Error prevention and recovery

- The agent MUST make destructive or irreversible actions
  explicit, distinguishable from routine actions, and
  confirmable or undoable.
- The agent MUST prevent invalid states at the input boundary
  when possible, and show validation next to the control that
  caused it rather than only in a summary region.
- The agent MUST provide visible recovery paths (undo, retry,
  reset, cancel) for any flow where mistakes are plausible. The
  agent MUST NOT hide recovery only in documentation.

## Draft state and data safety

- For Web GUIs with user-entered state that would be costly to
  recreate, preserve draft state across reloads, accidental
  tab/browser closes, and restarts by default. When draft
  persistence would be unsafe, provide an explicit equally-safe
  recovery path; the agent MUST NOT silently discard the draft.

## Accessibility minimum

- Every interactive control MUST have an accessible name
  (visible label, `aria-label`, or equivalent platform
  attribute) and a visible focus indicator.
- The primary flow MUST be completable by keyboard alone, with
  a logical focus and reading order.
- The agent MUST NOT convey state, selection, validity, or
  required action through color alone; pair color with shape,
  weight, icon, text, or position.
- Text and meaningful non-text content MUST meet at least WCAG
  2.1 AA contrast (4.5:1 for normal text, 3:1 for large text
  and UI components).
- The agent MUST NOT remove or suppress the default focus ring
  without providing an equally visible replacement.

## Verification gates

For any GUI change, before reporting completion the agent MUST:

1. Complete a first-use walkthrough on each claimed environment
   and viewport, following only the visible UI from the top.
2. Take screenshots of every changed view at normal width and
   at a reduced width (roughly two-thirds), and review for
   overflow, clipping, unexpected wrapping, alignment, and
   missing required content or controls.
3. Verify keyboard reachability and visible focus for the
   primary flow.
4. Run a whole-screen plausibility pass: nothing visibly broken,
   misaligned, or duplicated.

If the primary flow or next action is not immediately
understandable in the first-use walkthrough, treat the work
as unfinished and iterate until first-use clarity is achieved.

Source: github:metyatech/agent-rules@HEAD/rules/global/identity-and-scope.md

# Identity and scope

## Definitions

- **User** — the human operator. Name: "metyatech".
- **User-controlled repository** — a repository owned by
  `metyatech` on GitHub, or any repository where `metyatech`
  holds authoritative write permission (verify with
  `gh repo view --json owner,viewerPermission`).

## User identity

- The user controls the GitHub user/org `metyatech`, the npm
  scope `@metyatech`, and all repositories under
  `github.com/metyatech/*`.
- Treat any external reference to `metyatech` as
  user-controlled unless direct evidence contradicts it.
- Resolve identity uncertainty with the `gh` CLI before acting.

## Authority on user-controlled repositories

- Inside a user-controlled repository, the agent MAY perform
  end-to-end operations: issues, pull requests, pushes to
  default branches, releases, package publishes, and repository
  administration.
- For account-wide instructions, treat every user-controlled
  repository as in scope. Repository creation, splitting, and
  deletion are permitted within that scope.
- When publishing, cloning, adding submodules, or splitting
  repositories, default to placement under `metyatech` ownership
  unless the user specifies a different owner.

## Authority on third-party repositories

- The agent MUST obtain an explicit per-repository request
  before any write action on a third-party repository, even
  when push access exists.

## Resolving target ambiguity

- When a user instruction names a target ambiguously, verify
  the intended canonical repository from the request's purpose
  before writing.
- Keep uncertain work isolated on a separate branch or worktree
  until the canonical target is confirmed.

Source: github:metyatech/agent-rules@HEAD/rules/global/persistent-tracking.md

# Persistent task and thread tracking

## Definitions

- **Actionable task** — a discrete piece of work to complete
  (`task-tracker`, "what to do").
- **Thread** — a single discussion topic or design decision
  (`thread-inbox`, "what was discussed").
- **Persistent stage** — on-disk task stages: `pending`,
  `in-progress`, `committed`, `released`, `done`. `pushed` is
  derived from upstream reachability of the `committed` event.
- **Thread status** — `active`, `waiting` (auto-set by
  `--from user`), `needs-reply`, `review`, `resolved`.
- The agent MUST NOT create threads for tasks already tracked
  by `task-tracker`.

## Installation

- If not installed, install via
  `npm install -g @metyatech/task-tracker` and
  `npm install -g @metyatech/thread-inbox`.

## Storage

- `.tasks.jsonl` MUST be committed to version control. The
  agent MUST NOT add it to `.gitignore`.
- Store `.threads.jsonl` in the workspace root by passing
  `--dir <workspace-root>` to every `thread-inbox` invocation.
  The agent MUST NOT commit `.threads.jsonl`; add it to
  `.gitignore` explicitly.

## Session-start check

- At the start of any session that may involve state-changing
  work, run `task-tracker check`.
- At the start of every session, run
  `thread-inbox inbox --dir <workspace-root>` and
  `thread-inbox list --status waiting --dir <workspace-root>`.
  Report findings before starting new work.

## When to record

- When an actionable task emerges, immediately record it with
  `task-tracker add`. Treat `task-tracker` as the authoritative
  cross-session tracker; session-scoped task tools MUST NOT
  replace it.
- In `mwt` repos, run `task-tracker add` and any
  `task-tracker update` that changes `.tasks.jsonl` from the
  owning task worktree so the tracker state lands in that
  task's commits. The agent MUST NOT write tracked task state
  from the seed checkout except for read-only inspection or
  after delivery when no task worktree exists.
- A thread MUST capture discussion topics, design decisions,
  and multi-session context to remember across sessions.
- Add a `--from user` thread message for any substantive user
  interaction (decisions, preferences, directions, questions,
  feedback, approvals); status auto-sets to `waiting`. Err on
  recording rather than omitting.
- Add a `--from ai` thread message for informational updates.
  Use `--status needs-reply` when asking the user a question
  and `--status review` when reporting completion for review.
- Record the user's actual words verbatim. Threads MUST read
  as a conversation transcript, not meeting minutes.

## Stage transitions and lifecycle

- When a task reaches `committed`, run
  `task-tracker update <id> --stage committed` immediately
  before `git add` and `git commit` so `.tasks.jsonl` is
  included in that closing commit. The agent MUST NOT create
  tracker-only follow-up commits to record `pushed`.
- When reporting a task as complete, state the lifecycle stage
  explicitly. The agent MUST NOT claim "done" while downstream
  stages remain incomplete.
- Resolve threads when the topic is fully addressed or the
  decision is implemented and recorded in rules. Periodically
  purge resolved threads.
- If a thread captures a persistent behavioral preference,
  encode it as a rule per `rule-system` and resolve the thread.

Source: github:metyatech/agent-rules@HEAD/rules/global/posture-and-delivery.md

# Agent posture, approval, and delivery

## Definitions

- **Direct mode** — invoked by the human user.
- **Delegated mode** — invoked by another agent.
- **In-scope work** — work implied by the request without
  expanding the stated goal.
- **Delivery chain** — the ordered follow-on actions required
  to complete a request end to end.
- **Terminal state** — the strongest justified completion
  state available under the agent's authority.
- **Irreducible blocker** — a condition the agent cannot
  resolve deterministically without user input.

## Default posture

- Optimize for minimal human effort and default to automation.
- Prefer correctness, safety, robustness, verifiability, and
  maintainability over speed.
- Before acting, identify the exact user-visible effect and the
  real system surface that produces it. The agent MUST NOT
  substitute proxy actions for authoritative state change.
- The agent MUST NOT introduce backward-compatibility shims,
  legacy aliases, or temporary fallbacks unless the requester
  explicitly asks. Remove discovered legacy paths at the
  source in the same change set.
- Distinguish (1) instructions about how the agent should
  operate, (2) background context about the user's workflow or
  environment, (3) actual requirements for the artifact. The
  agent MUST NOT turn (1) or (2) into product features, scope,
  or UI requirements unless the user explicitly asks.

## Resolving uncertainty

- Resolve any uncertainty that can be settled deterministically
  through inspection, testing, or other reliable checks.
- Escalate only when uncertainty depends on user-only judgment.
  The agent MUST NOT fill those gaps with its own default.
- Make scope, risk, cost, and irreversibility decisions
  explicit when material. When inferring intent beyond literal
  wording, state the inference and matching next step.

## Approval

- In user-controlled repositories, the user's in-scope request
  is plan approval. Proceed with implementation, testing,
  commits, pushes, releases, and deploys without re-asking.
- For user-owned publishable packages, "commit and push" or
  "complete this fix" approves the normal release/publish
  chain.
- In delegated mode, the act of delegation itself is plan
  approval. The delegated agent MUST NOT re-request human
  approval; if scope must expand, fail back to the delegator.
- The agent MUST request explicit approval before destructive
  or hard-to-reverse actions, third-party account side effects,
  scope expansion, or actions whose impact cannot be bounded
  from inspection alone.

## Natural delivery chain

- After any user instruction, execute the delivery chain until
  terminal state or an irreducible blocker.
- For user-owned publishable packages, treat release and publish
  as in-scope unless the user explicitly opts out.

## Focus discipline

- Stay focused on the current task until terminal state or
  irreducible blocker, unless the user explicitly switches.
- The agent MUST NOT pause at intermediate milestones, treat
  partial satisfaction as completion, or context-switch merely
  because other tasks are visible.
- When delegated agents are running and no other in-scope local
  work exists, wait for completion or the next material state
  change rather than polling without progress.

## PR review feedback

- The agent MUST NOT remain idle on a failing pull request
  when a known automated fix exists. Apply the fix, re-verify,
  commit, and push.
- PR review feedback is pre-approved. Run the review loop until
  no actionable feedback remains or an irreducible blocker
  requires user input. See `pr-review-workflow`.

Source: github:metyatech/agent-rules@HEAD/rules/global/quality-and-verification.md

# Quality and verification

GUI verification lives in `gui-standards`. Procedural detail
lives in the `quality-workflow` skill.

## Definitions

- **State-changing work** — any change that modifies repository
  contents, deployed services, published artifacts, persisted
  data, or external accounts.
- **Acceptance criterion (AC)** — a binary, testable statement
  of a required outcome.
- **Critical-system journey** — an end-to-end user path through
  authentication, billing, authorization, persistence, or any
  other critical system whose failure carries direct user harm.

## Non-negotiable gates

For any state-changing work or any claim of "done", "fixed",
"working", or "passing":

1. Before starting, list AC as binary, testable statements.
2. Before each git commit, run the repository's full
   verification suite and observe success.
3. With each AC, define the evidence (concrete commands,
   outputs, or manual steps) that will demonstrate satisfaction.
4. Before concluding, enumerate the changed externally visible
   contract surface and map each changed surface item to direct
   verification evidence. Contract surface includes documented
   commands, subcommands, flags, API endpoints, UI controls,
   views, states, file formats, and other user-visible behavior.
5. For code or runtime changes, add automated tests. A bug fix
   MUST include a regression test.
6. Run the repository-standard verify command. If none exists,
   add one in the same change set.
7. Maintain AC and evidence internally; surface them only when
   the user requests.

## Failure handling

- The agent MUST NOT swallow errors. Fail fast with explicit
  context. Validate configuration and external inputs at system
  boundaries.
- CI and commit hooks MUST enforce full verification. Code
  fixes MUST follow a failing-test-to-passing-test loop.
- On a failing security audit, attempt the documented automated
  fix (`npm audit fix` or equivalent). If the fix succeeds and
  verification passes, commit and push.

## Runtime verification

- For user-facing apps, multi-client systems, multi-environment
  systems, or persisted-state systems, define the claimed
  environment matrix up front and verify every claimed primary
  path and state before concluding. Report evidence per claimed
  client, environment, and path. Any path not directly verified
  MUST stay unclaimed.
- For a critical-system journey, completion REQUIRES live or
  production-like end-to-end verification; unit, integration,
  build, and health checks alone are insufficient.
- Runtime verification MUST cover interruption, retry, reload,
  invalid-input, and stale-state behavior in addition to the
  happy path.
- If an intended environment cannot be exercised, stop short
  of completion, state the exact gap, and leave that
  environment unclaimed.

## Bug handling

- On every user-reported bug, identify the earliest
  deterministic gate that should have caught it and add or
  strengthen that gate in the same change set. A fix without a
  new catching gate is incomplete.
- When a bug reveals that a changed externally visible contract
  item lacked direct verification, the agent MUST add a
  mechanical contract check or matrix that covers the affected
  surface class rather than only the observed instance.
- When fixing a bug, the agent MUST identify the broader
  failure pattern that made the bug possible and extend the
  change so same-pattern failures are prevented by
  construction, by a shared invariant boundary, or by a
  generalized gate. A fix that only patches the observed
  instance is incomplete unless the remaining pattern is
  irreducible and explicitly reported.
- The agent MUST NOT claim bug-free behavior. Report scope,
  evidence, and residual risk.

Source: github:metyatech/agent-rules@HEAD/rules/global/release-and-publication.md

# Release and publication

Detailed release procedures live in the `release-publish` skill.

## Packaging hygiene

- Every published artifact MUST include a LICENSE file with
  `metyatech` as the copyright holder.
- The agent MUST NOT ship build artifacts, test artifacts, or
  local configuration files. Verify a clean environment can
  install and use the product via the README's documented
  steps.
- Define a SemVer policy and document what counts as a
  breaking change.
- Keep the package version and the Git tag consistent.
- Run dependency security checks before every release.

## Trusted publishing

- For public npm packages published from GitHub Actions, use
  npm trusted publishing (OIDC) instead of long-lived npm
  tokens whenever the package's npm registry supports it. The
  publish workflow MUST run on a Node and npm runtime version
  that satisfies trusted-publishing requirements.

## GitHub repository metadata

- For every public repository, set the GitHub Description,
  Topics, and Homepage fields. Topics MUST be assigned from the
  standard set defined in the `release-publish` skill.

## Verification of published artifacts

- Before reporting a publishable-package change as complete,
  verify the full delivery chain end-to-end: commit → push →
  version bump → release → publish → install verify.
- Verify that the published package resolves and runs correctly
  from a clean install before reporting completion.

## New repository bootstrap

- When work is intentionally isolated into a new user-owned
  repository because that repository is the canonical home for
  the work, bootstrap it as a real Git repository, create the
  GitHub remote, and push the initial history before reporting
  completion. Skip this only when the user explicitly opts out.

Source: github:metyatech/agent-rules@HEAD/rules/global/rule-system.md

# Rule and skill system

## Compliance vocabulary

RFC 2119 / RFC 8174 keywords carry their RFC meanings.
Omitted keywords mean MUST.

## Composition

- The agent MUST NOT edit `AGENTS.md` directly. It is composed
  output and MUST be self-contained at the repository root.
- A new repository MUST include `agent-ruleset.json`, compose
  AGENTS.md, and satisfy the required global standards before
  being reported complete.
- compose-agentsmd procedures live in
  `compose-agentsmd/tools/tool-rules.md`.

## Authoring rules

- Rules MUST be MECE: each obligation appears in exactly one
  module. Cross-reference rather than duplicate.
- Each rule MUST be atomic, imperative, use explicit or
  implied compliance keywords, and be testable as a yes/no
  check without outside context.
- Rules MUST NOT contain hedges such as "ideally", "reasonable",
  or "perhaps". Replace them with explicit conditions.
- Placement: `rules/global/` for any-workspace rules;
  `rules/domains/<domain>/` only for opt-in domains;
  `agent-rules-local/` only when no other repository will need
  it.
- When updating a rule, encode the underlying general
  principle, not the incident-specific surface example. Trace
  each update back to the reasoning error that permitted the
  original mistake.
- Persistent user instructions about future agent behavior MUST
  be encoded in the appropriate rule module in the same change
  set unless explicitly scoped to the current task.
- Treat rule/skill gaps, redundancy, misplacement, and
  recurring review feedback as systemic; fix the underlying
  issue and similar instances in the same change set.
- In delegated mode, the agent MUST NOT modify rules directly;
  report rule-gap suggestions to the delegator.
- Session memory resets between sessions. Persistent
  behavioral knowledge MUST live in rules; rules are the
  source of truth.

## Mechanisation of rules

- When a rule's compliance can be verified deterministically
  from the source or build artefacts (AST shape, prop
  presence, lexical patterns, file paths, schema conformance,
  numeric bounds), the agent MUST implement the check in a
  lint, schema, type system, or hook rather than as a written
  rule. Deterministic checks MUST run in the authoring
  pipeline (dev server, pre-commit, or CI) so violations
  surface without relying on human or agent recall.
- Written rules MUST be limited to (a) principles explaining
  why a system check exists, (b) judgement-based guidance
  that no deterministic check can capture, and (c) short
  cross-references to the system that enforces the rule.
- Before adding a new rule, the agent MUST decide whether it
  is mechanically checkable. If it is, the agent MUST ship
  the check in the same change set; text alone is not a
  complete delivery.
- When editing an existing written rule, the agent MUST
  migrate any mechanically checkable subset into a system
  check in the same change set and reduce the remaining text
  to the judgement-only residue.
- Skills follow the same division: reserve `SKILL.md` for
  principles and judgement guidance; push every deterministic
  check into the matching platform package, linter, or
  schema.

## Authoring skills

- A skill MUST follow the Agent Skills open standard.
- `SKILL.md` frontmatter MUST contain only `name` and
  `description`; `name` MUST be lowercase alphanumeric with
  hyphens and at most 64 characters; `description` MUST explain
  trigger conditions.
- `SKILL.md` MUST be platform-agnostic, use intent-level
  wording, and omit platform-specific tool names. Such
  examples MUST live in `README.md`.
- `SKILL.md` and `README.md` SHOULD be in English. Reference
  content MAY use the user's language when preserving direct
  linkage to user-facing artifacts.
- Skill instructions MUST be concise and action-oriented.
  Normative claims SHOULD use the compliance vocabulary.
- A skill MUST NOT duplicate AGENTS.md global rules; reference
  the rule module by name instead.

## Skill packaging and updates

- Each skill MUST live in its own repository with `SKILL.md`
  at the repository root. User-managed installable skills MUST
  live in public `metyatech/skill-<name>` repositories. Each
  skill repository MUST include a LICENSE file (MIT preferred)
  and MUST default to public visibility.
- The GitHub `metyatech/skill-*` repository is the canonical
  source of truth. Installed copies under
  `~/.agents/skills/<name>` are derived mirrors.
- The agent MUST NOT edit installed skill copies. Edit the
  source repository, push, and re-install via
  `npx skills add metyatech/skill-<name> --yes --global`.

Source: github:metyatech/agent-rules@HEAD/rules/global/sub-agent-delegation.md

# Sub-agent delegation and dispatch

## Definitions

- **Delegator** — the spawning agent or human.
- **Delegated agent** — a spawned agent in delegated mode.
- **Restricted operation** — modifying rules, merging or
  closing pull requests, creating or deleting repositories,
  releasing or deploying, force-pushing, or rewriting
  published history without explicit per-call delegation.
- **Tier** — task difficulty/cost classification.
- **Effort** — model reasoning intensity.

## Tier classification

- Tier routing: `sub-agent-dispatch`.

## Spawning a delegated agent

- The delegating prompt MUST state delegated mode, approval
  state, acceptance criteria, verification requirements, and
  task context. The delegator MUST NOT assume prior convo
  access.
- The delegating prompt MUST NOT restate rules already present
  in AGENTS.md.
- Two or more agents MAY write in the same repository only
  with isolated checkouts and one integration owner.
  Otherwise run sequentially.

## Delegated-agent obligations

- Respond in English and report evidence concisely.
- The delegated agent MUST NOT modify rules directly; report
  rule gaps for delegator review.
- Inherit the delegator's repository scope and MUST NOT expand
  it. If unable to operate within scope, fail back
  explicitly.
- If the delegated agent reports a read-only/no-write
  constraint, run a minimal reversible OS-temp probe and
  report the exact failure verbatim.
- A delegated agent MUST NOT perform a restricted operation
  without an explicit per-call delegation.

## Dispatch tooling

- Sub-agents MUST use `agents-mcp`; the agent MUST NOT use
  platform built-in subagent spawners.
- Before spawning, run `ai-quota`. If it is unavailable or
  fails, report and MUST NOT spawn.
- When spawning a sub-agent, explicitly specify both `model`
  and `effort` from the `sub-agent-dispatch` skill. The agent
  MUST NOT rely on defaults.
- Implementation sub-agents MUST use `mode: 'edit'`; default
  `mode: 'plan'` is read-only.
- After spawning, return to the user immediately and use
  background or non-blocking monitoring; the agent MUST NOT
  block waiting for completion.
- Dispatch details live in `sub-agent-dispatch`.

## Orchestrator model selection

- Orchestrator model defaults live in `sub-agent-dispatch`. The
  agent MUST NOT use elevated effort to improve rule
  compliance.

## Verification of sub-agent results

- The agent MUST NOT trust completion claims without evidence.
  Implementation sub-agents MUST return AC, evidence, files
  changed, assumptions, and risks.
- After implementation, run repo verify.
- If verification fails, cannot run, or the task is Heavy or
  release/production, spawn a separate reviewer with the AC
  and spec; require `PASS`/`FAIL`.
- The agent MUST NOT adopt a result as done unless reviewer
  status is `PASS`. Standard tier MAY skip reviewer with
  passing verify and clear AC evidence.

## Cost, execution, and lifecycle

- Use the minimum reasoning effort that reliably works. Prefer
  newer models at lower effort and MUST NOT use Heavy-tier
  models for Light or Standard work.
- The agent MUST NOT rapidly respawn sub-agents for the same
  task while one is still running without errors.
- After a team completes, shut down all team agents and clean
  up resources. If a sub-agent fails, retry or escalate.
- If a delegated task fails repeatedly because of quota limits,
  update the `task-tracker` stage so work resumes from the
  last successful stage.

Source: github:metyatech/agent-rules@HEAD/rules/global/writing-and-documentation.md

# Writing and documentation

## User responses

- Respond to the user in Japanese unless the user explicitly
  requests another language.
- Report commit and push activity only when the turn changed
  files.
- In direct mode, emit the Windows `SystemSounds.Asterisk`
  sound after completing a response. In delegated mode, the
  agent MUST NOT emit any sound. A manager agent MUST emit the
  sound at most once for the overall task.
- When delivering a new tool, feature, or artifact, explain
  what it is, how to use it, and its key capabilities.
- Keep progress reporting short and user-centric. Prefer long
  uninterrupted execution blocks; explain internals only when
  the user requests them.
- The agent MUST NOT include command transcripts in normal user
  reports unless the user explicitly requests them.
- At the end of a session or task, report any lingering
  unresolved technical friction or environment issues.

## Developer-facing writing

- Write developer documentation, code comments, commit
  messages, pull-request descriptions, and rule modules in
  English.

## README and documentation

- Every repository MUST include a README.md that covers:
  purpose, supported environments, setup/usage/development
  commands, required environment variables and configuration,
  release and deployment steps when applicable, and links to
  standard companion documents.
- For any change, assess documentation impact and update every
  affected doc in the same change set so documentation matches
  behavior. Omit no-op documentation notes from normal
  completion reports.
- For CLI tools, document every parameter with a description
  and at least one example invocation, plus at least one
  end-to-end example command.
- The agent MUST NOT include user-specific local paths, fixed
  workspace directories, drive letters, or personal data in
  documentation examples; prefer repository-relative paths and
  placeholders.

## Markdown linking

- When a Markdown document links to another local file, the
  link path MUST be relative to the Markdown file's location.
