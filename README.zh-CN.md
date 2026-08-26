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

## 示例

脱敏示例位于 [`examples/`](examples/)，无需 API Key 或数据库即可阅读。

## License

本项目采用 [MIT License](LICENSE)。

