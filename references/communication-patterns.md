# Communication Patterns

Use this file only when example orchestration phrasing is helpful.

These are drafting aids, not mandatory scripts.

## Kickoff Update Pattern

Use when first entering the workflow:

- state that orchestration mode is active
- state the first authority-reading or framing step
- avoid pretending implementation has already started

## Planner Handoff Pattern

Use when planner mode is active:

- state that planner owns primary decomposition judgment
- state the authority and code surfaces planner should read
- attach the matching role-doc path and require read-before-execution
- ask for packet boundaries, sequencing constraints, and risks

## Worker Packet Pattern

Use when dispatching a writable packet:

- state role
- state scope
- state write boundary
- state non-goals
- state required verification and return format
- attach the matching role-doc path and require read-before-execution

## Progress Update Pattern

Use when informing the user during long-running orchestration:

- report what stage is in progress
- report what is still being adjudicated
- avoid implying recovery or closure unless that decision was actually made

## Final Handoff Pattern

Use when the task reaches closure-ready state:

- summarize merged outcome
- report verification status
- report reviewer status
- report residual risks or unresolved but accepted issues
