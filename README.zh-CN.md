# Eval-Driven Iteration（评测驱动迭代）

> 当 AI 指标下降时，不要先调参，先做诊断。

一套面向 AI、Agent、RAG、Text2SQL、检索和分类系统的证据优先评测方法，帮助团队区分模型问题、检索问题、评测逻辑问题和基础设施问题。

## 为什么值得使用

这套方法源于真实的 AI 系统迭代过程：仅凭一个总分无法解释改动是否有效，因此将建立基线、保存逐条运行记录、提出可证伪假设、最小化修复和分层回归整理成可迁移的工作纪律。

## 三条原则

1. **证据优先。** 低分只是现象，不是根因。
2. **一次改动，一个假设。** 保持结果可归因、可回滚。
3. **保护分母。** 对齐样本和配置，基础设施失败单独统计。

## 快速了解

**修改前：** 看到某阶段失败率很高，就立即修改模型或 Prompt。

**修改后：** 先检查逐条运行记录，确认原始输出是否正确、错误发生在哪个阶段，再决定修复评测逻辑还是修改模型。

## 核心流程

建立基线 → 逐条记录 → 按阶段归因 → 提出可证伪假设 → 最小修复 → 回归已知失败 → 回归已知成功 → 检查独立样本 → 全量评测。

## 安装

将仓库目录放入 Claude 或 Codex 的 skills 目录，并保留：

```text
eval-driven-iteration/SKILL.md
```

Windows 下 Codex 常见路径为：

```text
%USERPROFILE%\\.codex\\skills\\eval-driven-iteration\\SKILL.md
```

英文主 Skill 见 [`SKILL.md`](SKILL.md)，中文参考版见 [`SKILL.zh-CN.md`](SKILL.zh-CN.md)。

## 安装后怎么调用

安装完成后通常不需要特殊命令。直接在任务中明确要求使用该 Skill 即可。推荐复制下面这段话：

```text
请使用 eval-driven-iteration Skill 处理这次评测。先检查基线和逐条运行证据，按阶段归类失败并提出可证伪假设，暂时不要修改代码。然后提出最小、可隔离的修复方案，并设计包含已知失败、已知成功和独立样本的回归验证。
```

指标异常模板：

```text
请使用 eval-driven-iteration 诊断指标为什么从 A/B 变成 C/D。必须使用同一批样本和相同分母，单独排除基础设施失败，先输出有证据的归因报告，再提出修复方案。
```

版本对比模板：

```text
请使用 eval-driven-iteration 对比版本 A 和版本 B。不要直接调参，先检查配置是否一致、逐条结果、失败分类和样本量是否足以支持结论。
```

Prompt 或检索修改模板：

```text
请在修改 Prompt/检索器前使用 eval-driven-iteration：先定位失败阶段，写出一个可证伪假设，说明哪些行为不能退化，再设计已知失败和已知成功回归。
```

在 Claude.ai 中，如果当前账号没有启用自定义 Skill，可以把 `SKILL.md` 作为附件上传到对话中。Claude Code 和 Codex 则把目录放入对应的 skills 路径后重新开启会话；也可以随时直接说“请使用 eval-driven-iteration”。

## 示例

脱敏示例位于 [`examples/`](examples/)，无需 API Key 或数据库即可阅读。

## License

本项目采用 [MIT License](LICENSE)。
