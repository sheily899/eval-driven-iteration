# Eval-Driven Iteration

一套用于 AI 系统评测与迭代的通用工作方法，帮助团队用可核对的运行记录和可证伪假设定位问题，再做最小修复，避免被漂亮但不可靠的指标误导。

## 为什么需要它

这套方法源于一次真实的 AI 系统迭代过程：团队发现仅凭一个总分无法解释改动是否有效，于是把“建立基线、记录证据、分层诊断、逐步回归”整理成一套可迁移的纪律。它适用于任何需要比较模型、Prompt、检索器或工具链的项目，不要求使用特定框架或数据集。

这里的“运行记录”指一次请求从输入到最终输出过程中留下的结构化信息，例如调用了哪些模块、产生了什么输出、耗时多久以及最终如何判定；英文资料中常称为 *trace*。本 Skill 不要求团队采用某一种日志系统。

## 核心流程

建立基线 → 逐 case 记录 trace → 按阶段分类失败 → 提出可证伪假设 → 最小修复 → 已知失败回归 → 已知成功回归 → 独立样本检查 → 全量评测。

详细规范见 [`SKILL.md`](SKILL.md)。

## 它和普通评测有什么不同

| 普通评测做法 | Eval-Driven Iteration |
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

**Before：** 汇总报告显示某个阶段失败率很高，团队立即修改该阶段的模型或 Prompt。

**After：** 先核对结构化运行记录，发现原始输出已经正确，错误发生在后续处理和结果归类；团队先修正评测逻辑，再决定是否需要修改模型。

两个案例都提供了脱敏输入、trace 摘要和前后归因，可在 [`examples/`](examples/) 中复核。

**Before：** 看到指标下降，就同时修改多个参数和多个模块。

**After：** 先提出一个可以被数据推翻的假设，只做一项最小改动，再分别用已知失败、已知成功和独立样本验证。

## 适用范围

适用于 AI/Agent/RAG/Text2SQL 的评测和迭代，不是普通单元测试的强制流程。网络和模型具有随机性时，最终标签和标准化结果集应作为幂等性核心；耗时、token 和重试次数应单独报告，允许自然波动。

## 示例

脱敏示例位于 [`examples/`](examples/)：

- [`routing-attribution.md`](examples/routing-attribution.md)
- [`candidate-noise.md`](examples/candidate-noise.md)

## License

本项目采用 [MIT License](LICENSE)。
