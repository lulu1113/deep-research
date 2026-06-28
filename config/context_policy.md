# Context Policy

本文件是 deep-research 在 Hermes / Codex 中执行复杂调研时的上下文治理规则。

核心原则：**Context Is All You Need**。不要让主 session 变成搜索日志、安装日志、失败尝试、无关 PDF 和反复争论的垃圾场。

---

## 1. 主 session 只保留调度信息

主 session 可以包含：

- 用户目标；
- research contract；
- 关键 artifact 摘要；
- 最终决策；
- 待办事项。

主 session 不应包含：

- 完整搜索日志；
- pip / npm / apt 安装长日志；
- 数据集下载日志；
- 长篇 PDF 原文；
- 无关网页全文；
- 多个被否 idea 的长篇讨论；
- 反复 debug 的所有历史尝试。

---

## 2. 每个阶段必须产出 artifact

复杂任务至少拆成：

```text
research_contract.md
source_scout.md / literature_scout.md
evidence_map.json
baseline_card.md        # 如适用
idea_card.md            # 如适用
result_review.md        # 如适用
final_report.md
qa.md
```

下一阶段只读取必要 artifact，而不是继承上一阶段完整聊天上下文。

---

## 3. 外部信息进入上下文前必须压缩

网页、论文、PDF、代码仓库进入主 session 前，需要先压缩成：

- source id；
- title；
- url / local path；
- why relevant；
- key claims；
- evidence strength；
- uncertainty；
- direct quotes or exact data points, when necessary；
- not enough information / gap。

不要把全文直接塞进主 session，除非任务明确要求逐字分析。

---

## 4. 日志进入上下文前必须摘要化

执行安装、下载、复现、训练、测试时：

- 原始日志保存到文件；
- 主 session 只接收 compact summary；
- 错误摘要必须包含 command、exit code、last relevant error、suspected cause、next action；
- 不允许把几千行日志直接贴入调研上下文。

---

## 5. Idea review 必须隔离

每个 idea 单独评审。

reviewer 只能看到：

- 当前 idea；
- research contract；
- baseline card 或对比框架；
- 必要证据摘要。

reviewer 不应看到：

- 其它 idea；
- idea 生成过程；
- 其它 reviewer 的结论；
- 主 session 的长聊天历史。

---

## 6. Research contract 不得事后静默修改

实验、调研或报告开始后，不允许为了适配结果修改 success signal、failure signal、评价指标或证据标准。

如果必须修改，创建新版本：

```text
research_contract_v2.md
```

并记录：

- 修改项；
- 修改原因；
- 修改发生在看到哪些结果之前或之后；
- 对最终结论的影响。

---

## 7. Claim-level trace 是硬要求

最终报告中的每个关键 claim 必须能追溯到：

```text
claim → evidence → source / local file / experiment result → confidence
```

对于缺少证据的结论，只能写成：

- 假设；
- 推断；
- 可能性；
- 待验证问题；
- data gap。

不能写成确定事实。

---

## 8. 公开模式和本地模式隔离

`/research-public`：

- 可以联网；
- 可以使用 SearXNG；
- 可以使用 Scrapling；
- 不能包含内部敏感信息。

`/research-local`：

- 不能联网；
- 不能调用 SearXNG；
- 不能调用 Scrapling；
- 只能读取用户指定本地路径。

---

## 9. 失败处理

如果上下文已经污染，例如：

- 连续多轮 debug 无进展；
- 多个 idea 混在一起；
- 模型持续引用错误假设；
- 结果解释开始偏离 contract；

应停止当前 session，输出 clean handoff：

```text
目标：
已确认事实：
未解决问题：
可复用 artifact：
不要继承的错误路径：
下一步：
```

然后开启新 session 继续。
