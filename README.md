# Eval-Driven Iteration

一套用于 AI、Agent、RAG 和 Text2SQL 系统的评测迭代纪律，帮助团队先用 trace 和可证伪假设定位问题，再做最小修复，避免被漂亮但不可靠的指标误导。

## 为什么需要它

这套方法是在一个真实的 Text2SQL 评测项目中，经过多轮检索、Prompt、SQL 执行和网络故障诊断后整理出来的。它专门解决几类常见问题：把评测 harness 的 bug 当成模型问题、只修好几个已知 case、用不同分母比较版本，以及把网络超时混入能力指标。

## 核心流程

建立基线 → 逐 case 记录 trace → 按阶段分类失败 → 提出可证伪假设 → 最小修复 → 已知失败回归 → 已知成功回归 → 独立样本检查 → 全量评测。

详细规范见 [`SKILL.md`](SKILL.md)。

## 它和普通评测有什么不同

| 普通做法 | Eval-Driven Iteration |
|---|---|
| 看到指标下降就调 Prompt | 先按 Router、Retrieval、Agent、执行等阶段定位 |
| 只重跑失败 case | 同时回归已知成功 case，防止修复引入退化 |
| 直接比较两个总分 | 先对齐 case 集合、环境和分母 |
| 把超时算成模型能力失败 | 单独统计 `infra_invalid`，不混入能力指标 |
| 一次修改多个参数 | 一次只验证一个可证伪假设 |
| 只保留汇总百分比 | 保留逐 case trace、证据和归因链 |
| 只追求分数变高 | 允许结论是“停止继续调参”或“数据不足以判断” |

## 安装

### Claude.ai / Claude Code

将本仓库目录作为 Skill 放入 Claude 的 skills 目录，确保文件路径为：

```text
<skills目录>/eval-driven-iteration/SKILL.md
```

Claude Code 重新启动或刷新 Skill 后即可使用。不同安装方式的目录位置以当前 Claude 文档为准。

### Codex

将整个目录复制到 Codex 用户 skills 目录：

```text
<CODEX_HOME>/skills/eval-driven-iteration/SKILL.md
```

例如 Windows 常见位置是 `%USERPROFILE%\\.codex\\skills\\eval-driven-iteration\\SKILL.md`。重新打开 Codex 会话后，涉及 AI 评测、指标异常或 Prompt/检索调优的任务即可按本 Skill 执行。

## Before / After

**Before：** 汇总报告显示 96.2%“路由失败”，立即开始调路由器。

**After：** 先检查 trace，发现路由器原始输出其实是 `database_query`；真正的问题是下游缺字段异常被兜底逻辑错误归因成“路由失败”，于是先修评测归因，而不是误改路由器。

两个案例都提供了脱敏输入、trace 摘要和前后归因，可在 [`examples/`](examples/) 中复核。

**Before：** 看到字段 Recall 下降，就同时放大 Top-K、修改 Rerank 阈值并改 Prompt。

**After：** 先离线比较候选池，确认正确字段是在 RRF 截断前被挤出，单独验证 Top-K 调整，再用已知成功 case 检查是否引入噪声。

## 适用范围

适用于 AI/Agent/RAG/Text2SQL 的评测和迭代，不是普通单元测试的强制流程。网络和模型具有随机性时，最终标签和标准化结果集应作为幂等性核心；耗时、token 和重试次数应单独报告，允许自然波动。

## 示例

脱敏示例位于 [`examples/`](examples/)：

- [`routing-attribution.md`](examples/routing-attribution.md)
- [`candidate-noise.md`](examples/candidate-noise.md)

## License

本项目采用 [MIT License](LICENSE)。
