# Waiting And Recovery

Read this document when any delegated packet is in flight and the next action is waiting, progress-checking, recovering, or reclaiming ownership.

## Read When

- planner, worker, reviewer, or `delegate-orchestrator` is in flight
- you are considering a progress check
- you are considering recovery, interrupt, close, or ownership reclaim
- you need the worker-in-flight local boundary

## Skip When

- no child packet is currently in flight
- the current action is packet drafting, dispatch, or closure under another governing stage

## This Doc Governs

- waiting baseline
- progress checks
- recovery evidence
- ownership reclaim
- worker-in-flight local boundary

## Worker In-Flight Local Boundary

Once a writable worker packet is in flight, the main agent may prepare integration readiness but must not substantially design the next implementation wave before the current worker result returns and is adjudicated.

Before worker return, local notes about later waves should remain only:

- open seams
- open questions
- integration risks
- capability or truth-boundary constraints

If the main agent continues local work while a writable worker is in flight, briefly state that it is staying in integration-readiness mode and limiting itself to those note types.

## In-Flight Control

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

- once a write surface has been delegated, the main agent must not keep editing that same write surface in parallel
- do not disguise ownership reclaim as a time-based failure judgment
