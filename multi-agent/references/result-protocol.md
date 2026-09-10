# 结果协议

每个子 Agent 都应返回：

```yaml
status: completed | blocked | needs_followup
summary: 简洁说明任务结果
changes:
  - 修改过的文件或状态；只读任务留空
evidence:
  - 关键路径、发现、命令和观察结果
validation:
  passed: [已通过的检查]
  not_run: [未执行的检查及原因]
risks:
  - 尚未解决的风险或假设
next_action: 可选的具体后续动作
```

证据必须足够具体，使编排器无需重演子 Agent 的推理就能核实结果。不得声称执行过实际上没有运行的编译、测试或 UI 检查。报告阻塞时，必须说明缺少的输入、授权或依赖，或者反复出现的具体故障。

`reviewer` 应使用按严重程度排序的问题列表代替 `changes`；适用时，每个问题都要包含严重程度、文件与行号、影响和简洁的修复方向。`tester` 必须包含准确的命令或 UI 检查，并区分由本次修改引起的失败与环境限制。
