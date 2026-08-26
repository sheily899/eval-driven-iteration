# Candidate selection example

配套文件：[`input.json`](candidate-noise/input.json)、
[`trace-before.json`](candidate-noise/trace-before.json)、
[`trace-after.json`](candidate-noise/trace-after.json)。这些文件是脱敏的最小离线演示。

## 现象

扩大候选集合后，覆盖率上升，但精确率和最终任务成功率下降。

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

逐条对照新增候选与最终输出，发现绝大多数新增候选没有被后续模块使用，说明候选扩展规则过宽。

## 最小实验

先离线模拟相关性门槛和总上限，再用已知有效输入验证关键候选没有被过滤。只有离线结果支持后，才把参数变更带入模型评测。示例中新增候选从 37 个降至 8 个，实际使用率从 2.7%（1/37）提升到 25.0%（2/8）。

## 可迁移经验

Recall 上升不等于系统变好。候选池扩张必须同时观察 Precision、Agent 实际引用率和最终执行准确率，并保持失败 case 与成功 case 的分层回归。
