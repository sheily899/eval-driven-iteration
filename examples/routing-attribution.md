# Output attribution example

配套文件：[`input.json`](routing-attribution/input.json)、
[`trace-before.json`](routing-attribution/trace-before.json)、
[`trace-after.json`](routing-attribution/trace-after.json)。这些文件是脱敏的最小演示，不连接任何模型或数据库。

## 现象

评测汇总把 77/80 条标记为“入口判断失败”。

## 输入

```json
{"case_id":"demo-001","query":"How many customers joined in 2024?"}
```

## 诊断

## 原始输出

逐条运行记录显示，原始入口判断其实已经选择了正确的处理路径。失败发生在后续数据准备步骤；异常被通用兜底逻辑重新标成了入口失败。

Before trace 摘要：

```json
{"route":{"action":"database_query","confidence":0.95},"error":"missing schema field","final_label":"routing_failure"}
```

After trace 摘要：

```json
{"route":{"action":"database_query","confidence":0.95},"error":"missing schema field","final_label":"retrieval_failure"}
```

## 错误归因

将后续处理异常错误归到了入口判断阶段。

## 证据

原始处理路径和置信度正确；错误字段出现在后续模块。

## 最小修复

先修正归因优先级：读取原始处理路径并区分后续异常；再用应当成功的输入和应当失败的输入验证。修复前后的比较使用同一批输入和相同分母：入口失败从 77/80 降为 3/80，另有 71/80 被重新归入后续阶段。

## 修复后输出

入口失败从 77/80 降为 3/80，另有 71/80 被重新归入后续阶段。

## 不能推出的结论

这个案例证明评测归因逻辑存在问题，但不能证明模型能力已经提升。

## 可迁移经验

不要只看最终标签。任何批量、整齐划一的失败分布，都应先检查原始 trace 和评测 harness。
