# Planner-First

Read this document when planner-first mode is active or when you are deciding whether it should be.

## Read When

- you are deciding whether planner mode is needed
- planner-first mode is active
- planner has been dispatched and you are considering any local exception before planner return
- planner has returned and you are adjudicating whether to follow or challenge its decomposition

## Skip When

- the task is not using planner-first mode
- planner has already been adjudicated and the next action is dispatch, waiting, or closure under another governing stage

## This Doc Governs

- planner-owned judgments
- pre-planner local boundary
- post-planner adjudication
- planner output expectations

## Planner-First Protocol

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

Default after planner dispatch:

- wait for planner return
- do not continue local probing just because idle time exists
- do not re-check already-settled authority, task, or routing material without a new reason

Only-if-needed local exceptions before planner return:

- check `git status`
- check `git diff --stat`
- use `rg`, file names, or path-level search to confirm one bounded surface fact
- confirm authority, task, or routing at route-level only when that fact is newly uncertain
- detect one obvious contradiction that affects packet safety or validity
- record one open question or one blocking feasibility risk that the planner packet did not already settle
- draft or tighten the planner packet only if dispatch has not happened yet
- read `1-2` very small code snippets only to answer one explicit bounded question that cannot safely wait

Use route-only code orientation by default before planner return.

That means:

- prefer waiting over local probing when planner output can safely answer the question
- prefer search hits, path signals, dirty-worktree metadata, and route-level document confirmation when a narrow exception is actually needed
- do not read broad code bodies just to become generally informed
- do not let feasibility checking turn into early implementation analysis

Before taking any local exception, explicitly state:

- what bounded question or safety fact is being checked
- why that check is needed before planner return
- why planner output cannot safely answer it later

Before any pre-planner snippet read, also explicitly state:

- which snippet points are the maximum allowed read in this pre-planner step

Not required before planner return:

- authority re-check when authority is already established
- task or routing re-check when the planner packet already rests on settled routing
- `git status`, `git diff --stat`, or `rg` as default hygiene with no concrete question

Not allowed before planner return:

- multi-file large-window code reading
- whole-file front-window reads used as orientation
- de facto implementation review
- candidate-seam analysis that effectively chooses the implementation direction
- a de facto worker plan
- a preferred implementation shape
- a preferred packet structure treated as already decided
- mainline writable execution just because the likely direction feels obvious

Node trigger when entering planner-first mode: explicitly state that the default after planner dispatch is to wait, and that any local exception must be justified by a bounded cannot-wait question rather than by idle time.

### Post-Planner Adjudication

After planner return:

- use planner output as the default basis for packet drafting and sequencing
- review only the minimum surface needed to confirm a disagreement, contradiction, or omission
- do not reopen planner-scale reading just to form a second local plan unless a clear gap exists

### Planner Output

Planner output should stay concise and practical. Cover:

- task understanding
- proposed packet boundaries
- what stays with the main agent
- sequencing constraints
- review and verification hot spots
