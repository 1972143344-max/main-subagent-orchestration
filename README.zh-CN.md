# Main/Sub-Agent Orchestration

[English](./README.md)

这是一个面向 Codex 的技能仓库，用于让主 agent 以编排者而不是主要实现者的方式组织工作。它支持可选规划、边界清晰的 worker 分派、主线集成，以及独立最终审查。

## 仓库内容

- `SKILL.md`：编排规则与执行约束
- `agents/openai.yaml`：技能入口元数据
- `roles/worker.md`：worker 基线行为
- `roles/reviewer.md`：reviewer 基线行为
- `roles/planner.md`：planner 基线行为
- `references/worker-packet-template.md`：可选的 worker packet 草稿模板

## 适用场景

当任务适合由主 agent 负责读权威、切分边界、协调执行和组织验证，而不是自己吞下主要实现工作时，可以使用这个技能。

这个技能围绕几条稳定原则组织：

- 先读 authority，再进入执行
- 按 ownership 切分，不按文件数切分
- 尽量保持 worker 的 write surface 不重叠
- worker 返回前先做自审
- 有独立 reviewer 时，把统一最终审查交给 reviewer
- 角色文档保持轻量，只提供稳定基线

## 安装

把本仓库目录复制到 Codex 的 skills 目录下，并命名为 `main-subagent-orchestration`。

目录结构应为：

```text
<CODEX_HOME>/skills/main-subagent-orchestration/
```

## 说明

- 这个技能刻意保持通用，不依赖某个特定仓库结构。
- 角色文档是 task packet 的补充层，不替代 packet 本身。
- worker packet 模板只是参考，不是强制包装格式。
