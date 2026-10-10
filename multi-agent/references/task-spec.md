# TaskSpec 子任务规范

委派任务时使用以下简洁模板。没有实际信息的字段可以省略。

```yaml
id: 简短且唯一的任务名
capability: explorer | worker | specialist | reviewer | tester
model_route:
  tier: fast | balanced | strongest
  model: 当前会话中的实际模型 ID，或 inherit
  reasoning_effort: low | medium | high | xhigh
  source: profile | task_override | user_override | inherit
  reason: 一句话说明选择依据
objective: 一个可观察的目标结果
context:
  - 仅列出此任务需要的事实和路径
scope:
  read: [允许读取的路径或区域]
  write: [允许写入的路径或区域] # 只读 Agent 留空
depends_on: [依赖的任务 ID]
constraints:
  - 项目约束或用户要求
acceptance:
  - 可观察的完成条件
validation:
  - 期望执行的检查，或“报告建议执行的检查”
budget:
  scope: small | medium | large
  stop_when: 明确的停止条件
```

创建子 Agent 时，可以把模板写成简洁的自然语言或 YAML。必须明确说明该 Agent 是否允许修改文件，并要求其保留用户无关修改、按照结果协议返回。

`model_route` 必须在创建子 Agent 前填写。`model` 应来自当前运行时实际提供的模型列表与 `routing.allowed_models` 的交集，不能凭记忆猜测；运行时不能覆盖模型时，只有确认主 Agent 模型处于允许范围内才填写 `inherit`，否则不创建子 Agent。

`model_route.source` 记录路由来自当前配置、任务风险调整、用户指定还是运行时继承。它用于解释成本决策，不表示硬性 token 限额。

合理的任务边界应遵循所有权和数据流。例如：先调查 API 契约，再实现生产者，最后在契约明确后更新依赖它的消费者。不要仅仅因为几个强耦合方法位于不同文件，就把它们拆成同时执行的任务。
