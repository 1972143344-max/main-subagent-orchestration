# Dispatch And Delegation

Read this document when the current action is drafting, dispatching, or authorizing delegated packets.

## Read When

- you are drafting or dispatching first-layer packets
- you are deciding what may stay local
- you are deciding `fork_context`
- you are deciding `sub-delegation policy`
- you are considering a `delegate-orchestrator` packet

## Skip When

- the current action has not reached dispatch
- the current action is purely waiting, recovery, or closure under another governing stage

## This Doc Governs

- packet boundaries
- first-layer packet exclusivity
- dispatch completeness
- `fork_context`
- `delegate-orchestrator`
- `sub-delegation`

## Dispatch And Packeting

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
