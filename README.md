# Eval-Driven Iteration

> **When an AI metric drops, do not tune first. Diagnose first.**

An evidence-first workflow for teams building and evaluating AI, Agent, RAG, and data-query systems.

It helps you distinguish:

- model or prompt failures
- retrieval and ranking failures
- evaluation-logic mistakes
- infrastructure and availability failures

## Why this is worth using

This method grew out of repeated real-world AI evaluation cycles where a single score could not explain whether a change helped. It turns that experience into a portable discipline: establish a baseline, preserve per-request evidence, form falsifiable hypotheses, make one minimal change, and verify the result before scaling up.

## Three principles

1. **Evidence before intuition.** A low score is a symptom, not a root cause.
2. **One change, one hypothesis.** Keep improvements attributable and reversible.
3. **Protect the denominator.** Align cases and configurations; report infrastructure failures separately.

## See it in 30 seconds

**Before:** A team sees a high failure percentage and immediately changes the model prompt.

**After:** The team checks the per-request record, finds that the model output was correct but the evaluator misclassified a downstream error, fixes the evaluator, and only then reassesses model quality.

Try the self-contained examples (no API key or database required):

```bash
git clone https://github.com/sheily899/eval-driven-iteration.git
cd eval-driven-iteration
open examples/
```

For Chinese documentation, see [`README.zh-CN.md`](README.zh-CN.md).

## 核心流程

建立基线 → 逐 case 记录 trace → 按阶段分类失败 → 提出可证伪假设 → 最小修复 → 已知失败回归 → 已知成功回归 → 独立样本检查 → 全量评测。

详细规范见英文主 Skill [`SKILL.md`](SKILL.md)，中文参考版见 [`SKILL.zh-CN.md`](SKILL.zh-CN.md)。

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
