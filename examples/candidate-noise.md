# Candidate noise example

配套文件：[`input.json`](candidate-noise/input.json)、
[`trace-before.json`](candidate-noise/trace-before.json)、
[`trace-after.json`](candidate-noise/trace-after.json)。这些文件是脱敏的最小离线演示。

## 现象

结构性 Join 补全后，字段 Recall 上升，但 Precision 和 SQL 执行准确率下降。

输入：

```json
{"case_id":"demo-002","query":"List customer names and order totals"}
```

Before trace 摘要：

```json
{"retrieval":{"top20_fields":20,"gold_recall":0.70,"precision":0.40},"completion":{"added_fields":37,"used_in_sql":1},"execution":{"accuracy":false}}
```

After trace 摘要：

```json
{"retrieval":{"top20_fields":20,"gold_recall":0.68,"precision":0.55},"completion":{"added_fields":8,"used_in_sql":2},"execution":{"accuracy":true}}
```

## 诊断

逐条对照补入字段与最终 SQL，发现绝大多数补入字段没有被 Agent 使用，说明“只要表中有一个字段进入候选就补全全部 Join 键”的规则过宽。

## 最小实验

先离线模拟表级 Top-N 门槛和每题总上限，再用已知阳性 case 验证真正需要的 Join 键没有被过滤。只有离线结果支持后，才把参数变更带入模型评测。示例中补入字段从 37 个降至 8 个，实际 SQL 引用率从 2.7%（1/37）提升到 25.0%（2/8）。

## 可迁移经验

Recall 上升不等于系统变好。候选池扩张必须同时观察 Precision、Agent 实际引用率和最终执行准确率，并保持失败 case 与成功 case 的分层回归。
