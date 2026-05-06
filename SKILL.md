---
name: main-subagent-orchestration
description: Orchestrate coding work through a main agent that reads authority first, optionally uses a planner for high-context decomposition, decomposes a non-trivial task into bounded parallel sub-agent packets, requires worker self-review, and routes the final unified review to an independent reviewer agent when available. Use when the user explicitly wants sub-agents, delegation, or a workflow like "main agent orchestration + optional planner + sub-agent parallel implementation + total review closure", and wants the lead agent to minimize direct implementation work.
---

# Main/Sub-Agent Orchestration

Use this skill when the user wants a lead agent that behaves as an orchestrator rather than the primary implementer, or when the task clearly benefits from that orchestration shape without conflicting with higher-priority instructions.

The main agent owns task framing, authority checks, decomposition, worker assignment, integration, and verification orchestration. For high-context tasks, the main agent may first delegate global analysis to a planner agent that reads the authority layer, proposes decomposition, and highlights risks before worker packets are drafted. Workers own bounded implementation packets and must self-review before returning. When a reviewer agent is available, the main agent should route the final unified review to that independent reviewer instead of acting as the final reviewer itself.

Treat this skill as an orchestration guide, not a rigid messaging protocol. The main agent should decide the exact shape, order, and wording of sub-agent communication based on the task, risk, and repo surface.

## Use This Skill When

- the user explicitly asks for sub-agents, delegation, or parallel agent work
- or the task clearly benefits from orchestrated delegation with bounded ownership seams and does not conflict with higher-priority instructions
- the task spans multiple modules or surfaces and has clear ownership seams
- the user wants the lead agent to reduce direct implementation and focus on orchestration
- the work needs an explicit final review after parallel implementation
- the task has enough authority or cross-module context that planning itself should be isolated from implementation

## Do Not Use This Skill When

- the task is small enough for one focused pass
- the write surface is too entangled to split safely
- the next critical-path action is a single urgent blocker that the main agent should handle directly
- the user asked for one focused audit without parallel delegation

## Main Agent Contract

When preparing to delegate, the main agent will often benefit from checking the following:

1. Reconfirm the active task, the user's constraints, and the authoritative docs for the current repo.
2. Read the minimum authority layer needed: `AGENTS.md`, governance/spec/plan docs, task history, and any directly affected modules.
3. Decide whether planning should stay local or be delegated to a planner agent because the task carries a large global-context burden.
4. Build a concrete architecture map and identify safe split points.
5. Decide what must stay local on the critical path and what can be delegated in parallel.
6. If a planner agent is used, treat its output as guidance for decomposition and context shaping rather than a rigid script that workers must follow mechanically.
7. Prefer doing coordination, integration, and verification orchestration locally; avoid taking a major implementation packet unless delegation is unsafe or blocked.
8. If a reviewer agent is available, plan from the start to hand the integrated result to that reviewer for the unified final review.

These checks are default orchestration guidance. They do not require a fixed prompt shape, fixed handoff order, or a single mandatory communication template. The main agent may probe locally first, stage the work in waves, or defer parts of this checklist when that better matches the task.

## Execution Cost Check

Before substantial execution, identify whether the task has a large execution packet.

- Treat a packet as large when it is likely to consume unusually high context or execution effort.
- A packet usually counts as large when at least two of these are true:
  - it will require repeated runtime probing, command retries, or environment diagnosis
  - it will generate large raw logs, traces, or protocol evidence
  - it will force repeated back-and-forth across multiple modules or surfaces
  - it is likely to consume a large share of the main agent's context window or tool budget

Treat planning as a separate high-context packet when the global analysis itself is large enough that it would dilute the quality of worker packets or main-agent orchestration.

## Dispatch Before Execution

Before or during substantial execution, it is often useful to classify the work into:

- authority and planning
- optional planner packet, if planning has a large global-context burden
- large execution packet, if any
- sidecar packets
- integration and final review

If a large execution packet exists, assign it to a worker by default before the main agent spends too long executing it locally. It is still acceptable to do a small amount of probing first when that is the clearest way to discover the real split.

## Allowed Main-Agent Local Work

The main agent may keep small, low-context packets locally.

- authority reading and repo triage
- planner dispatch and planner-output review when planner mode is used
- decomposition and worker-packet drafting
- small deterministic glue changes
- integration work after workers return
- unified verification orchestration
- narrow post-review fixes

Being on the critical path is not, by itself, a reason for the main agent to keep the large execution packet.

## High-Context Pressure Bias

When the main agent is already carrying substantial context pressure, it should bias even more strongly toward orchestration and delegation rather than absorbing additional implementation work locally.

Signals of high-context pressure may include:

- multiple unfinished sub-agent packets
- substantial accumulated authority, diff, or runtime evidence already held by the main agent
- a likely upcoming need for integration, verification orchestration, or final review
- signs that continued local execution would force more broad code reading, runtime probing, or repeated retries
- any situation where further local implementation would materially increase the risk of context drift or compression before closure

Under this condition, the main agent should keep implementation local only when the packet is truly small and low-risk.

A packet should be treated as truly small and low-risk only when it is all of the following:

- local to a narrow ownership area
- deterministic and unlikely to require iterative probing or retries
- free of shared-contract or cross-packet implications
- unlikely to require substantial new authority or code reading
- easy to verify in a short local loop
- unlikely to create extra integration burden if handled locally

If those conditions are not clearly met, the default under high-context pressure is to delegate the packet to a sub-agent and keep the main agent focused on coordination, staging, integration, and verification orchestration.

## Delegation Rules

- Split by ownership boundary, not by arbitrary file count.
- Give each worker a disjoint write set whenever possible.
- Before dispatching more than one writable sub-agent packet, explicitly compare their intended write surfaces. If overlap remains, re-split the packets, stage them in waves, or downgrade one of them to read-only before dispatch.
- Once a write surface has been delegated to a sub-agent, the main agent must not keep editing that same write surface in parallel.
- If the main agent must reclaim a worker-owned write surface to unblock the critical path, it must first interrupt or close the corresponding sub-agent, then explicitly take ownership of that surface before continuing.
- State explicit non-goals so workers do not widen scope.
- Pass only the task-local context needed for that packet.
- When a matching role document exists under `roles/`, the main agent must explicitly remind the delegated sub-agent to read the corresponding role document, such as `roles/<role>.md`, for baseline behavior before executing the task packet. Do not rely on the sub-agent to infer this on its own.
- Let the main agent decide the exact sub-agent message contents. Constrain the task and boundaries, but do not over-specify wording or force one packet format when the task would benefit from a different handoff shape.
- Prefer giving workers a strong starting point and boundary, not a frozen search space. Workers may explore within their ownership area and should escalate only when they need to cross shared contracts, shared state, or another packet's boundary.
- Default sub-agent model to the current model used in the active user conversation. Keep the default reasoning effort at `high` or `xhigh` when available. The main agent may choose `medium` for a narrow, low-risk, and deterministic sub-agent packet when that better fits the task. If the user explicitly requests a different model or reasoning effort for a sub-agent packet, follow the user's instruction.
- If one packet depends on another packet's output, keep the blocker local or stage the work in waves instead of pretending it is parallel.
- When a large execution packet exists, assign it to a worker by default unless it is truly small and deterministic or delegation is unsafe or blocked.

## Role Documents

## Role Resolution

Resolve the delegated sub-agent role by its primary responsibility in the packet, not by superficial cues such as how many files it reads or whether it mentions review.

- Use a `worker` role when the packet's primary job is to implement, modify, or verify a bounded change inside an assigned write boundary.
- Use a `reviewer` role when the packet's primary job is read-only review: finding defects, regressions, risky assumptions, weak validation, or documentation drift without directly fixing them.
- Use a `planner` role when the packet's primary job is read-only planning, decomposition, dependency analysis, or sequencing guidance rather than implementation.
- Use an `explorer` role when the packet's primary job is read-only evidence gathering, repo exploration, or environment inspection to support a later implementation or review packet.

Choose the role by the packet's dominant responsibility. Do not classify a packet as `worker`, `reviewer`, `planner`, or `explorer` based only on whether it reads code, mentions review, or touches many files.

Once the role is resolved, check whether a matching role document exists under `roles/`. If it does, the dispatch must include the explicit read-the-role-doc reminder required by this skill.

Role documents under `roles/` are a reusable baseline layer for delegated sub-agents.

- The main agent should decide at dispatch time whether a delegated role has a corresponding role document and, when it does, the dispatch must contain an explicit reminder for the sub-agent to read it before execution.
- This reminder should stay generic and role-driven rather than hard-coding one specific role such as reviewer, planner, or worker into the orchestration contract.
- The role document supplements the task packet; it does not replace packet-specific scope, non-goals, deliverables, or higher-priority authority.
- Keep role-document use lightweight. It should improve consistency without turning delegation into a mandatory ceremony.
- A dispatch that omits this reminder when a matching role document exists is incomplete and should be corrected before send.
- The exact reminder wording is flexible, but omission is not: if a matching role document exists, the dispatch should contain an explicit read-the-role-doc reminder.

## Optional Planner Mode

Use planner mode when the task carries a large authority surface, multiple plausible split points, or a high risk of cross-packet conflict if workers infer the plan locally.

Planner mode is optional. Do not force it onto small or already-obvious tasks.

### Planner Responsibilities

- read the relevant authority and current-task materials
- build the global task picture before implementation packets are assigned
- suggest a safe decomposition and identify what should remain with the main agent
- mark cross-packet dependencies, shared contracts, and likely conflict areas
- identify the minimum high-value context each worker should start from
- flag review hot spots and verification priorities
- surface uncertainty or challenge shaky task framing back to the main agent when a plan-level assumption looks weak or incomplete

### Planner Non-Goals

- do not implement the task
- do not replace the main agent's judgment about dispatch, staging, or integration
- do not write a brittle step-by-step script for workers unless the task is unusually fragile
- do not over-constrain what a worker may read inside its ownership boundary
- do not treat the first decomposition idea as final when new evidence suggests a better split

### Planner Output Shape

Planner output should stay concise and practical. Cover:

- task understanding
- proposed packet boundaries
- what stays with the main agent
- packet dependencies or sequencing constraints
- worker starting context and key files or modules
- review and verification hot spots

Use this output as a decomposition aid, not as a rigid protocol.

## Required Worker Instructions

However the main agent chooses to communicate, every worker packet should make these constraints clear:

- you are not alone in the codebase
- do not revert edits made by others
- stay within the assigned ownership boundary
- self-review before returning
- report changed files, tests run, and residual risks
- surface uncertainty instead of silently widening scope

Workers should usually treat the main agent as the adjudication point for boundary questions, unclear assumptions, or suspected upstream mistakes. This is a flexibility aid, not a mandatory pause-before-action rule: workers may still make small local decisions inside their ownership area.

Use `references/worker-packet-template.md` only as an optional drafting aid when it helps. Do not treat it as a mandatory wrapper.

## Main Agent Review Loop

After workers return:

1. Read each result and inspect the changed files.
2. Resolve cross-worker contract mismatches before running broad verification.
3. Run the required tests and quality gates on the integrated tree.
4. If a reviewer agent is available, ask that reviewer to independently review the current integrated result. The main agent should not present itself as the final reviewer in that case.
5. Reviewer dispatch does not end the main agent's responsibility for the task. The main agent remains responsible for driving the task forward, coordinating any further packets needed, and adjudicating the reviewer outcome.
6. The main agent may continue coordinating additional worker, explorer, or planner packets as needed before closure.
7. However, the main agent must not enter closure, finalize governance/task records, or present the task as complete until the reviewer has returned and the reviewer outcome has been adjudicated.
8. If no reviewer agent is available, perform the total review locally for correctness, regressions, security boundaries, missing tests, architecture drift, and doc drift.
9. If review or adjudication leaves blocking issues unresolved, continue execution or investigation rather than closing. Keep the fix local only when the remaining work still clearly qualifies as tiny, local, and deterministic under the current context state. When high-context pressure is already present, this bias toward re-delegation should be even stronger. Do not close on top of known integration defects.
10. Closure is allowed only when the review-adjudicated outcome is one of:
   - no blocking defects remain, or
   - remaining blockers have been explicitly surfaced, judged acceptable to leave unresolved for this task, and reported as residual risk.
11. Close with explicit verification status, reviewer completion status, adjudication status, and residual risks.

Reviewer feedback is not limited to code defects. A reviewer may also challenge planner assumptions, packet boundaries, integration choices, or other upstream decisions when they appear to weaken the result. The main agent remains responsible for adjudicating those challenges and deciding whether to re-split, revise, or proceed.

## Default Communication Pattern

Prefer this communication order when it helps, but deviate freely when the task calls for a different orchestration shape:

1. short kickoff update with the first authority-reading step
2. concise decomposition plan with worker ownership, and planner involvement when used
3. progress updates as workers run and as integration happens
4. final handoff with merged outcome, verification, and residual risk

## Practical Guardrails

- Re-check authority after context compression, handoff, or long-running parallel work.
- Keep planner output high-signal and lightweight; if the planner becomes a second full orchestrator or dumps generic prose, collapse back to direct main-agent decomposition.
- Keep worker prompts concrete enough that integration does not depend on guesswork.
- Do not let the skill's suggested communication flow override a clearly better task-specific orchestration choice by the main agent.
- Do not turn planner mode into a mandatory ceremony. If the split is already obvious, skip it.
- Do not confuse worker context shaping with hard visibility limits. The goal is to reduce noise, not to forbid local exploration inside the assigned boundary.
- Allow planner, worker, and reviewer agents to push uncertainty or challenges back to the main agent without turning that feedback path into a mandatory approval loop for every small decision.
- Do not confuse a worker self-review with the unified final review. Both are required.
- When a reviewer agent is available, do not let the main agent silently substitute itself for that final unified review unless the user explicitly asks for that exception.
- When the repo uses governance docs, update task records and audits in the same round as closure, but only after review adjudication permits closure.
- If the main agent notices it has drifted into a high-context execution loop that should have been delegated, stop, re-decompose, and hand the remaining large execution packet to a worker instead of continuing by inertia.
