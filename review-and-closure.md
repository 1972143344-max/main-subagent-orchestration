# Review And Closure

Read this document when integrated review begins, reviewer findings return, or the task is approaching closure.

## Read When

- workers have returned and integrated review is beginning
- reviewer-loop sequencing is beginning
- reviewer has returned
- you are adjudicating post-review fixes
- you are considering closure readiness or final broad gates

## Skip When

- the current action has not yet entered review, post-review adjudication, or closure
- the current action is still inside planner-first, dispatch, or waiting under another governing stage

## This Doc Governs

- main review loop
- reviewer truth
- post-review fix adjudication
- closure readiness
- final broad gates and final docs

## Review And Closure Loop

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

Node trigger before any post-review write: explicitly state that reviewer has returned, name the `fix owner`, the `allowed write scope`, and why the remaining work does or does not satisfy the tiny-fix threshold.

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
