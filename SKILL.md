---
name: main-subagent-orchestration
description: Orchestrate coding work through a main agent that reads authority first, optionally uses a planner for high-context decomposition, decomposes a non-trivial task into bounded sub-agent packets, may explicitly grant one bounded internal lane to a `delegate-orchestrator`, requires worker self-review, and routes the final unified review to an independent reviewer agent when available. Use when the user explicitly wants sub-agents, delegation, or a workflow like "main agent orchestration + optional planner + sub-agent implementation + total review closure", and wants the lead agent to minimize direct implementation work while retaining top-level closure authority. If this skill is triggered and adopted as the governing workflow for the current action, read the full SKILL.md before the first major orchestration action.
---

# Main/Sub-Agent Orchestration

Use this skill when the lead agent should behave primarily as an orchestrator rather than the primary implementer.

## G0. Front Door

### Use This Skill When

- the user explicitly asks for sub-agents, delegation, or parallel agent work
- the task spans multiple modules or surfaces with meaningful ownership seams
- the user wants the lead agent to minimize direct implementation and focus on orchestration
- the work needs an explicit final review after parallel implementation
- planning itself carries enough global context that it should be isolated from implementation
- the main agent may need to hand one bounded internal lane to a locally autonomous `delegate-orchestrator` while keeping top-level ownership and closure authority

### Do Not Use This Skill When

- the task is small enough for one focused pass
- the write surface is too entangled to split safely
- the next critical-path action is a single urgent blocker the main agent should handle directly
- the user asked for one focused audit without delegation

### First Rule After Trigger

Once this skill is adopted as the governing workflow for the current action:

1. read this main `SKILL.md`
2. identify your current role identity
3. identify whether planner mode is needed
4. identify which companion document, if any, governs the next major orchestration action

Do not dispatch planner, worker, reviewer, explorer, or `delegate-orchestrator` packets based only on a partial front-window read of this skill.

### Major Orchestration Actions

Treat these as major orchestration actions that require the full main `SKILL.md` first:

- dispatching a planner packet
- dispatching any writable worker packet
- dispatching a `delegate-orchestrator` packet
- locking packet boundaries or ownership boundaries
- declaring planner-first sequencing
- entering reviewer-loop or closure-stage sequencing under this skill

### Skill Boundary

This skill governs:

- role ownership
- decomposition and packet boundaries
- planner authority
- delegation completeness
- waiting, recovery, and ownership reclaim
- review-loop and closure sequencing

This skill does not replace:

- repo authority docs
- repo-local quality gates
- role documents under `roles/`
- packet-specific scope and non-goals

## G0.5 Role Identity Gate

Before any major orchestration action under this skill, explicitly determine your current role identity in the current run.

Possible identities:

- `top-level main agent`
- `ordinary delegated child` such as `worker`, `planner`, `reviewer`, or `explorer`
- `delegate-orchestrator`

Then explicitly state:

- which role you are
- which authority you do have
- which authority you do not have
- whether you currently have packet-dispatch authority
- whether you currently have bounded local autonomy
- whether you still retain final closure authority

Default interpretation:

- only the `top-level main agent` owns top-level decomposition, first-layer packet dispatch, and final closure adjudication
- an `ordinary delegated child` does not inherit those authorities
- a `delegate-orchestrator` has only the bounded local autonomy explicitly granted by its parent packet and does not become a second top-level main agent

Node trigger before the first major orchestration action: explicitly state your current role identity and the authority boundaries that follow from it.

Read `roles/main-agent.md` when:

- you identified yourself as the `top-level main agent`
- and you are about to perform packet drafting, dispatch, waiting or recovery judgment, or closure work

Skip `roles/main-agent.md` when:

- you are an ordinary delegated child
- or you are a `delegate-orchestrator`
- or no major orchestration action is about to occur

## G1. Role Model And Core Contract

The orchestration model is main-agent-centered.

### `main agent`

Primary job:

- own task framing, authority alignment, decomposition decisions, packet dispatch, integration, and closure adjudication

Must do:

- read the authority layer needed for safe delegation
- decide whether planner mode is needed
- keep write ownership explicit
- coordinate review and closure

Must not do by default:

- absorb the large execution packet locally just because it is on the critical path
- silently overrule packet ownership after delegating a write surface
- substitute itself for an available independent reviewer at final review time

Typical inputs:

- user request
- repo authority and code context
- planner output
- worker returns
- reviewer findings

Typical outputs:

- packet definitions
- adjudication decisions
- integration decisions
- final verification and closure judgment

### `planner`

Primary job:

- own read-only global decomposition judgment when planning burden is high

Must do:

- read the relevant authority and current-task materials
- propose packet boundaries, sequencing, and risks
- identify what should remain with the main agent

Must not do:

- implement the task
- become a second top-level orchestrator
- replace the main agent's final dispatch or closure authority

Typical inputs:

- authority layer
- current task framing
- broad code and dependency context

Typical outputs:

- decomposition recommendation
- worker starting context
- conflict or risk map

### `worker`

Primary job:

- implement or verify a bounded change inside an assigned write boundary

Must do:

- stay inside the assigned boundary
- self-review before returning
- report changed files, tests run, and residual risks

Must not do:

- widen scope silently
- revert unrelated edits
- cross into another packet's ownership area without escalation

Typical inputs:

- packet scope
- non-goals
- write boundary
- role document reminder when applicable

Typical outputs:

- bounded implementation result
- self-review summary
- verification status

### `reviewer`

Primary job:

- perform independent read-only review of the integrated result

Must do:

- look for defects, regressions, risky assumptions, weak validation, and drift

Must not do:

- fix issues directly
- dispatch writable child agents

Typical inputs:

- integrated tree
- relevant authority
- review focus areas

Typical outputs:

- findings
- residual risks
- review completion status

### `explorer`

Primary job:

- perform bounded read-only evidence gathering to support later implementation or review

Must do:

- gather facts needed for a later packet

Must not do:

- convert itself into implementation
- claim broader write authority

### `delegate-orchestrator`

Primary job:

- own a bounded internal execution lane that is too large or too multi-surface for one narrow packet, while staying strictly inside a parent packet boundary granted by the main agent

Must do:

- execute only inside the granted parent packet boundary
- use local decomposition only to complete that bounded lane
- keep child ownership, write scope, and escalation triggers explicit
- integrate child results and return one closure-ready handoff to the main agent

Must not do:

- become a second top-level main agent
- rewrite the user goal, top-level task framing, or closure standard
- expand beyond the granted boundary
- silently grant itself broader autonomy than the packet explicitly allowed

Typical inputs:

- bounded parent packet
- explicit autonomy grant
- allowed child roles
- write policy
- escalation triggers
- return contract

Typical outputs:

- bounded internal packet plan
- child dispatches inside the granted boundary
- integrated result for that lane
- residual risks and escalation points

### Hard Boundary

Repeat this rule wherever delegation and recovery meet:

- once a write surface has been delegated, the main agent must not keep editing that same write surface in parallel

### Lifecycle Overview

The default lifecycle under this skill is:

1. trigger and adopt the orchestration workflow
2. determine role identity and authority boundary
3. decide whether planner-first is needed
4. dispatch bounded first-layer packets
5. wait, progress-check, recover, or reclaim based on evidence
6. integrate results, run reviewer-loop, adjudicate post-review fixes, and close only when closure-ready

## G2. Companion Routing

Use the documents below when the next major orchestration action enters their stage boundary.

### `planner-first.md`

- Read when:
  - you are deciding whether planner mode is needed
  - planner-first mode is active
  - planner has been dispatched and you are considering any local exception before planner return
  - planner has returned and you are adjudicating whether to follow or challenge its decomposition
- Skip when:
  - the task is not using planner-first mode
  - planner has already been adjudicated and the next action is dispatch, waiting, or closure under another governing stage
- This doc governs:
  - planner-owned judgments
  - pre-planner local boundary
  - post-planner adjudication
  - planner output expectations
- Node trigger:
  - before planner dispatch, or before any local exception while planner is still out, explicitly state whether `planner-first.md` now governs the action

### `dispatch-and-delegation.md`

- Read when:
  - you are drafting or dispatching first-layer packets
  - you are deciding what may stay local
  - you are deciding `fork_context`
  - you are deciding `sub-delegation policy`
  - you are considering a `delegate-orchestrator` packet
- Skip when:
  - the current action has not reached dispatch
  - the current action is purely waiting, recovery, or closure under another governing stage
- This doc governs:
  - packet boundaries
  - first-layer packet exclusivity
  - dispatch completeness
  - `fork_context`
  - `delegate-orchestrator`
  - `sub-delegation`
- Node trigger:
  - before any first-layer packet dispatch, explicitly state whether `dispatch-and-delegation.md` now governs the action

### `waiting-and-recovery.md`

- Read when:
  - planner, worker, reviewer, or `delegate-orchestrator` is in flight
  - you are considering a progress check
  - you are considering recovery, interrupt, close, or ownership reclaim
  - you need the worker-in-flight local boundary
- Skip when:
  - no child packet is currently in flight
  - the current action is packet drafting, dispatch, or closure under another governing stage
- This doc governs:
  - waiting baseline
  - progress checks
  - recovery evidence
  - ownership reclaim
  - worker-in-flight local boundary
- Node trigger:
  - before any waiting-state or recovery-state action, explicitly state whether `waiting-and-recovery.md` now governs the action

### `review-and-closure.md`

- Read when:
  - workers have returned and integrated review is beginning
  - reviewer-loop sequencing is beginning
  - reviewer has returned
  - you are adjudicating post-review fixes
  - you are considering closure readiness or final broad gates
- Skip when:
  - the current action has not yet entered review, post-review adjudication, or closure
  - the current action is still inside planner-first, dispatch, or waiting under another governing stage
- This doc governs:
  - main review loop
  - reviewer truth
  - post-review fix adjudication
  - closure readiness
  - final broad gates and final docs
- Node trigger:
  - before any post-review local write or any closure claim, explicitly state whether `review-and-closure.md` now governs the action

## G3. Role-Doc Routing

Use role documents as execution-boundary reinforcements, not as replacements for this main skill or for stage-specific companion docs.

### Main-Agent Reinforcement

Read `roles/main-agent.md` when:

- you are the `top-level main agent`
- and you are entering planner-first discipline, packet drafting, dispatch, waiting or recovery judgment, post-review adjudication, or closure

Skip `roles/main-agent.md` when:

- you are not the `top-level main agent`
- or no major orchestration action is about to occur

### Matching Role-Doc Requirement

If a matching role document exists under `roles/`, the packet must include:

- the role-doc path
- a start rule: read that role doc before execution

If those items are missing, the packet is incomplete and should not be dispatched.

Node trigger before dispatch: explicitly state that you are attaching the matching `roles/<role>.md` file and requiring read-before-execution.

The main agent should not preload role-document contents into its own context by default merely because those files exist.

## G4. Low-Frequency References

Use the items below only when the current action needs them.

### `references/worker-packet-template.md`

- Read when:
  - you want an optional drafting aid for a bounded worker packet
- Skip when:
  - packet drafting is already clear without a template
- This doc governs:
  - optional packet prompt shape
- Node trigger:
  - if you use it, explicitly state that it is a drafting aid rather than a mandatory wrapper

### `references/communication-patterns.md`

- Read when:
  - you want example kickoff, planner handoff, progress-update, or final-handoff phrasing
- Skip when:
  - the current wording is already clear without example patterns
- This doc governs:
  - optional phrasing examples only
- Node trigger:
  - if you use it, explicitly state that message examples do not override hard role, ownership, waiting, or closure rules
