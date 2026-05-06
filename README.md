# Main/Sub-Agent Orchestration

[中文说明](./README.zh-CN.md)

This repository contains a Codex skill for running a lead-agent workflow with optional planning, bounded worker delegation, integration, and independent final review.

## Included files

- `SKILL.md`: the orchestration contract
- `agents/openai.yaml`: prompt entrypoint metadata
- `roles/worker.md`: baseline implementation behavior
- `roles/reviewer.md`: baseline review behavior
- `roles/planner.md`: baseline planning behavior
- `references/worker-packet-template.md`: optional packet drafting aid

## What the skill is for

Use this skill when a task benefits from a lead agent that coordinates the work instead of acting as the primary implementer.

The skill is designed around a few stable ideas:

- read authority before execution
- split by ownership, not by file count
- keep worker write surfaces disjoint
- let workers self-review before handoff
- route the unified final review to an independent reviewer when available
- keep role documents lightweight and role-driven

## Install

Copy this directory into your Codex skills directory as `main-subagent-orchestration`.

Expected layout:

```text
<CODEX_HOME>/skills/main-subagent-orchestration/
```

## Notes

- The skill is intentionally generic. It does not assume a specific repository structure.
- Role documents are supplements to the task packet, not replacements for it.
- The worker packet template is optional. It is a drafting aid, not a required wrapper.
