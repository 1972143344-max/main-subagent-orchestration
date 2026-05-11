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
2. identify whether planner mode is needed
3. decide what must stay local, what can be delegated, and what must wait for planner adjudication

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

## G2. Planner-First Protocol

Use planner mode when the task has a large authority surface, multiple plausible split points, or a high risk of cross-packet conflict if workers infer the plan locally.

Planner mode is optional for small or already-obvious tasks.

### Planner-Owned Judgments

When planner mode is active, planner should remain the primary owner of mainline decomposition judgment.

Before planner return, the main agent should not substantially decide:

- preferred backend or execution lane
- preferred contract or continuation shape
- preferred implementation shape
- preferred module placement
- worker packet boundaries
- mainline stage order

If the main agent sees a strong candidate early, it may record that only as:

- a constraint
- an open question
- a feasibility fact
- a contradiction requiring adjudication

When entering planner-first mode, briefly state which judgments are reserved for planner and which narrow pre-planner local actions are still allowed before continuing.

### Pre-Planner Local Boundary

Before planner return, local work must stay framing-only, question-bounded, and non-directional.

Allowed pre-planner local work:

- confirm authority
- do lightweight codebase orientation
- identify candidate seams
- detect obvious contradictions
- draft the planner packet

Not allowed before planner return:

- a de facto worker plan
- a preferred implementation shape
- a preferred packet structure treated as already decided
- mainline writable execution just because the likely direction feels obvious

### Post-Planner Adjudication

After planner return:

- use planner output as the default basis for packet drafting and sequencing
- review only the minimum surface needed to confirm a disagreement, contradiction, or omission
- do not reopen planner-scale reading just to form a second local plan unless a clear gap exists

### Worker In-Flight Local Boundary

Once a writable worker packet is in flight, the main agent may prepare integration readiness but must not substantially design the next implementation wave before the current worker result returns and is adjudicated.

Before worker return, local notes about later waves should remain only:

- open seams
- open questions
- integration risks
- capability or truth-boundary constraints

If the main agent continues local work while a writable worker is in flight, briefly state that it is staying in integration-readiness mode and limiting itself to those note types.

### Planner Output

Planner output should stay concise and practical. Cover:

- task understanding
- proposed packet boundaries
- what stays with the main agent
- sequencing constraints
- review and verification hot spots

## G3. Dispatch And Packeting

This block defines what gets delegated, what may stay local, and what every packet must carry.

### Fork-Context Default

`fork_context:false` is the default hard rule for delegated packets.

Use this default to preserve sub-agent context isolation and keep the child focused on its own packet instead of inheriting the parent thread's broader orchestration state.

Use `fork_context:true` only when the packet cannot be made self-contained without losing task-critical local observations.

If `fork_context:true` is used, the main agent must explicitly state:

- that it is intentionally breaking the default context-isolation rule
- why the packet cannot be made self-contained
- which task-critical local observations require the full-history fork

Node trigger before dispatch: explicitly state whether `fork_context` is `false` for sub-agent context isolation or `true` as a justified exception.

### Execution Cost Check

Treat a packet as large when at least two of these are true:

- it will require repeated runtime probing, command retries, or environment diagnosis
- it will generate large raw logs, traces, or protocol evidence
- it will force repeated back-and-forth across multiple modules or surfaces
- it is likely to consume a large share of the main agent's context window or tool budget

When a large execution packet exists, assign it to a worker by default unless delegation is unsafe or the packet is truly tiny and deterministic.

### What May Stay Local

The main agent may keep only narrow, low-context work locally:

- authority reading and repo triage
- planner dispatch and planner-output adjudication
- packet drafting
- small deterministic glue changes
- integration after workers return
- unified verification orchestration
- truly tiny post-review fixes after explicit adjudication

Before dispatching a writable worker packet, briefly state the delegated write boundary and the local work that will remain with the main agent.

### High-Context Pressure Bias

When the main agent is already under substantial context pressure, bias even more strongly toward orchestration and delegation.

Implementation should stay local only when the packet is clearly:

- narrow
- deterministic
- single-boundary
- low-read
- easy to verify in a short local loop

### Role Resolution

Resolve delegated roles by dominant responsibility:

- `worker` for bounded implementation or writable verification
- `reviewer` for read-only review
- `planner` for read-only planning and decomposition
- `explorer` for read-only evidence gathering
- `delegate-orchestrator` for a bounded internal execution lane that the main agent explicitly wants to run with local autonomy inside a parent packet boundary

### First-Layer Packet Gate

When the top-level main agent dispatches first-layer packets, those packets must be mutually exclusive.

Do not send the same top-level mission to multiple first-layer agents and expect them to subdivide it correctly on their own.

In particular:

- first-layer packets must not restate the entire top-level task to multiple agents
- first-layer packets must already be partitioned at the top level into non-overlapping packet scopes
- if multiple first-layer agents are used, each one should receive only its own packet scope, not the full mission plus a hope that it will self-narrow correctly

Node trigger before first-layer dispatch: explicitly confirm that the first-layer packets are mutually exclusive and are not restating the same full mission.

### Dispatch Completeness

Every delegated packet should contain:

- role
- scope
- non-goals
- ownership or write boundary
- expected deliverable
- escalation path for boundary problems

If the delegated role is `delegate-orchestrator`, the packet must also contain:

- explicit autonomy grant
- parent packet boundary
- allowed child roles
- sub-delegation mode
- write policy
- escalation triggers
- return contract

### Role-Doc Dispatch Gate

If a matching role document exists under `roles/`, the packet must include:

- the role-doc path
- a start rule: read that role doc before execution

If those items are missing, the packet is incomplete and should not be dispatched.

Node trigger before dispatch: explicitly state that you are attaching the matching `roles/<role>.md` file and requiring read-before-execution.

The main agent should not preload role-document contents into its own context by default merely because those files exist.

### Autonomous Delegate Packets

`delegate-orchestrator` is not a default substitute for the existing four roles.

Use it only when:

- the lane is still bounded enough to fit one parent packet
- the lane is too multi-surface or too internally decomposable for one narrow worker, reviewer, planner, or explorer packet
- the main agent explicitly wants to hand down bounded local orchestration instead of retaining that decomposition work at the top level

Do not use it when:

- one narrow role packet is already sufficient
- the lane boundary is still unclear
- the packet would need to rewrite top-level sequencing or closure standards
- the real problem is broad uncertainty that should stay with planner or the main agent

Hard boundary:

- `delegate-orchestrator` may coordinate child packets only inside the granted parent packet boundary
- it does not become a second top-level main agent
- it does not inherit authority to change the user goal, broaden ownership, or replace final closure adjudication

Node trigger before dispatching a `delegate-orchestrator` packet: explicitly state that bounded local autonomy is being granted, and list the granted boundary, allowed child roles, write policy, escalation triggers, and return contract.

### Required Worker Constraints

Every worker packet should make these constraints explicit:

- you are not alone in the codebase
- do not revert edits made by others
- stay within the assigned ownership boundary
- self-review before returning
- report changed files, tests run, and residual risks
- surface uncertainty instead of silently widening scope

### Bounded Sub-Delegation

Sub-delegation is not a default behavior.

The top-level main agent must explicitly state each packet's `sub-delegation policy` at dispatch time.

If a packet does not explicitly state a `sub-delegation policy`, treat it as:

- `not authorized unless you escalate back`

Default interpretation:

- sub-delegation is an extension of the current delegated role
- child agents should therefore normally use the same role type as the parent sub-agent

There are only two valid authorization paths:

1. `pre-authorized`
   - the parent packet explicitly authorizes sub-delegation in advance
   - the allowed child role, read/write mode, boundary, and purpose should be stated
2. `authorized after escalation back`
   - the child agent discovers during execution that a child packet appears necessary
   - it must not spawn the child directly
   - it must escalate back to the parent or main agent first
   - the parent or main agent must explicitly approve the child dispatch before it happens

### Main-Agent Authorization Gate

The main agent may explicitly pre-authorize sub-delegation only when:

- the parent packet boundary is already stable enough to delegate safely
- the likely child work is expected to stay fully inside that boundary
- the allowed child roles, read/write mode, and escalation triggers can be stated up front
- the child need is structural and plausible at dispatch time, not just speculative parallelism
- top-level closure authority will still remain with the main agent

Do not explicitly pre-authorize sub-delegation when:

- the parent packet boundary is still unclear
- the possible child need is only hypothetical
- the likely child work may escape into shared-contract or cross-packet reshaping
- the parent packet should really be reframed as a different first-layer packet instead
- the grant would effectively create undeclared bounded autonomy that should instead be expressed through a `delegate-orchestrator` packet

Node trigger before granting `pre-authorized` sub-delegation: explicitly state why pre-authorization is warranted and list the allowed child roles, read/write mode, boundary, escalation triggers, and return expectations that will govern that child work.

Use sub-delegation when:

- there is a clear secondary sub-problem fully inside the parent packet boundary
- the child packet is needed to help complete the parent packet's current role task
- local completion would create meaningful context pressure, reading burden, or waiting cost inside the parent packet
- the child result still rolls into the parent packet's own deliverable instead of redefining it

Do not use sub-delegation when:

- there is no clear secondary boundary
- the real goal is broad parallelism rather than a bounded child packet
- the child would need to cross the parent packet boundary or widen ownership
- the child would convert a read-only packet into implementation by side effect
- the child dispatch would turn the parent packet into a de facto new top-level orchestrator without explicit authorization
- the parent packet is already small enough that local completion is cheaper and clearer

When sub-delegation is explicitly authorized:

- it must stay entirely inside the parent packet boundary
- writable child agents must stay inside the parent's write boundary
- broader ownership or reshaping must escalate back to the top-level main agent
- reviewer may use only read-only child agents
- if the parent packet or later approval explicitly authorizes bounded internal orchestration, the child role may differ from the parent role
- otherwise, keep the child as the same role type as the parent sub-agent

If the packet does not pre-authorize sub-delegation, the child agent must escalate back before spawning another agent.

Node trigger before dispatch: explicitly state the packet's `sub-delegation policy`.

Node trigger before approving child dispatch: explicitly state whether the child packet is:

- `pre-authorized`
- or `authorized after escalation back`

Node trigger before child dispatch: explicitly state why the child packet is needed, and whether the child is a same-role extension or an explicitly authorized bounded internal orchestration exception.

## G4. In-Flight Control

This block governs waiting, progress checks, recovery, and ownership reclaim.

### Waiting Baseline

For substantial `worker`, `reviewer`, and `planner` packets, a single waiting round should default to no less than `10 minutes`.

This is a patience floor, not a reclaim trigger.

### Progress Checks

Progress checks should usually be non-interrupt status requests.

Within one waiting round:

- default to at most one non-interrupt progress check unless a new concrete reason appears

A progress check should determine:

- what the sub-agent is currently doing
- whether it is still inside boundary
- what remains before closure-ready return
- whether there is a concrete blocker
- whether packet shape is wrong

Before sending a progress check or considering recovery, briefly state the current waiting rule and the specific evidence you are still trying to collect.

### Healthy Progress

Continue waiting when the sub-agent still provides:

- a concrete current step
- a clear description of what remains
- confirmation that it is still inside boundary
- a credible next step
- a coherent explanation for why the work is still in reading, comparison, testing, or implementation

Unchanged files, missing timestamps, and lack of landed edits are weak signals only.

### Recovery Evidence

Recovery may be justified only by stronger evidence such as:

- explicit tool or environment failure
- explicit blocked status
- packet-shape failure
- sustained no-progress after three or more rounds with weak or repetitive status

This is not a timeout problem by default. It is an evidence problem.

### Final Gate Before Recovery

Before interrupting or closing a packet, make one last explicit judgment.

If the latest signal still says, concretely:

- I am still implementing or reviewing
- I am still inside boundary
- I still have a coherent path to completion

then continue waiting.

If the latest signal says, concretely:

- I am blocked
- this packet cannot close inside boundary
- I must cross another ownership surface
- I cannot explain a credible next step

then recovery may be appropriate.

### Interrupt, Close, And Ownership Reclaim

`interrupt-now` and `close_agent` are recovery tools, not speed tools.

Use them only when:

- recovery evidence is present
- the write surface must be formally reclaimed
- the packet is mis-split or blocked
- the user or environment forced immediate recovery

Repeat this rule wherever delegation and recovery meet:

- do not disguise ownership reclaim as a time-based failure judgment

## G5. Review And Closure Loop

This block governs integration, reviewer-loop sequencing, post-review fixes, and final closure.

### Main Review Loop

After workers return:

1. inspect each result and changed file
2. resolve cross-worker contract mismatches
3. run only targeted pre-review verification needed for the current integration state
4. if a reviewer is available, dispatch independent review
5. keep broad final gates for the closure-candidate tree, not for a tree still inside a reviewer-triggered fix loop

### Reviewer Truth

Repeat this rule wherever worker self-review and reviewer-loop meet:

- worker self-review is required, but it is not the unified final review

When a reviewer is available, the main agent should not silently substitute itself for that final unified review unless the user explicitly asks for that exception.

Before entering unified review, briefly state whether reviewer-led final review is required for this run and why.

### Post-Review Fix Adjudication

After reviewer return, explicitly decide whether the fix stays local or is delegated.

That adjudication must state:

- `fix owner`
- `reason`
- `allowed write scope`

Local post-review fixes are allowed only when the remaining work is truly tiny, narrow, single-boundary, deterministic, and easy to verify in a short local loop.

If it does not clearly qualify as tiny, delegate a narrow follow-up worker packet.

Before choosing a local post-review fix, briefly state the `fix owner`, the reason, and why the remaining work does or does not satisfy the tiny-fix threshold.

### Closure Readiness

Do not enter closure while reviewer-found blocking work remains.

Closure is ready only when:

- no blocking defects remain, or
- remaining blockers were explicitly surfaced, judged acceptable for this task, and reported as residual risk

### Final Broad Gates And Final Docs

If the repo uses closure-stage broad quality gates such as `build`, `ci`, or equivalent, those gates must run and pass on the closure-candidate tree before final completed-state governance/task/doc synchronization is written.

Repeat this rule wherever review and closure meet:

- final completed-state documentation must not be written as a substitute for real closure readiness

If broad gates fail after completed-state docs were written, those docs are stale and must be corrected before closure is claimed.

## G6. Low-Frequency References

Use the items below only when the current action needs them.

### `references/worker-packet-template.md`

Use only as an optional drafting aid when it helps packet writing.

Do not treat it as a mandatory wrapper.

### `references/communication-patterns.md`

Use when you want example kickoff, planner handoff, progress-update, or final-handoff shapes.

Do not let message examples override the hard role, ownership, waiting, or closure rules in this main `SKILL.md`.
