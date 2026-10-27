# multi-agent

一个用于 Codex 的多 Agent 编排技能。遇到可以拆成独立工作流的复杂仓库任务时，它会规划子任务、选择模型、协调实现与审查，并汇总实际验证结果。

## 何时使用

- 需要并行调查多个模块或故障原因。
- 可以让不同 Agent 独立实现、审查或验证，且写入范围不重叠。
- 独立复核能显著降低高风险修改的遗漏。

小型、必须顺序推进或集中在同一强耦合区域的任务由主 Agent 直接处理。技能的自动发现不等于获得委派权限；实际创建子 Agent 仍受当前会话的运行时策略约束。也可以在任务中显式使用 `$multi-agent`。

## 模型与用量控制

编辑 [`multi-agent/config/model-routing.yaml`](multi-agent/config/model-routing.yaml) 中的 `active_profile`，可在 `economy`、`balanced`（默认）和 `quality` 之间切换。该文件还定义各角色的模型与推理强度、并发数量、总 Agent 数、轮次和上下文传递范围。

当前模型范围为 `gpt-6-luna`、`gpt-6.1-terra` 和 `gpt-6.1-sol`，最高使用 GPT-6.1 Sol。`routing.allowed_models` 同时约束默认路由、自动升降档和降级。各 profile 使用 Luna 处理轻量任务、Terra 处理适合的普通任务，并使用 Sol 处理复杂任务和高风险审查；具体角色和推理强度以配置为准。

`routing.task_overrides` 配置按任务类型选择的模型：当允许覆盖且运行时支持 Terra 时，低风险的视觉理解和界面操作任务可使用 Terra；风险下限仍由[路由策略](multi-agent/references/routing-policy.md)约束。

创建子 Agent 前，技能会报告计划使用的模型、推理强度和任务边界。配置中的数量限制用于控制编排规模，不是精确的 token 上限；模型不可用时会按[路由策略](multi-agent/references/routing-policy.md)说明降级原因。

## 安装

将仓库中的 `multi-agent` 目录复制到 Codex 的用户级技能目录（Windows 默认位置为 `%USERPROFILE%\.codex\skills\multi-agent`）。更新时覆盖该目录中的同名文件。技能入口为 [`multi-agent/SKILL.md`](multi-agent/SKILL.md)，UI 元数据位于 `multi-agent/agents/openai.yaml`。

仓库根目录的 README 供人阅读，不属于需要安装的技能目录。
