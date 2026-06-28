# Context-first Research Workflow

本文档把“Context Is All You Need”的经验落成 deep-research 的可执行工作流。

它适用于两类任务：

1. **公开资料/市场/技术调研**：例如半导体产业、竞品、标准、专利、技术路线。
2. **深度学习科研辅助**：例如文献调研、baseline 选择、idea 评审、实验 contract、结果分析。

本工作流的目标不是让 AI 写出更好看的文字，而是尽量保证：

- 结论有来源；
- 研究问题被清楚定义；
- idea 与证据、方法、实验或资料之间能一一对应；
- 不把搜索日志、失败安装日志、无关讨论塞进主上下文；
- 不允许事后根据结果修改成功标准；
- 不让多个 agent 在同一个污染上下文里互相放大幻觉。

---

## 1. 核心原则

### 1.1 Context Is All You Need

模型能力高度依赖上下文质量。上下文一旦被污染，模型会出现：

- 对旧错误路径产生路径依赖；
- 反复修不好简单 bug；
- 混淆多个 idea、多个 baseline 或多个客户项目；
- 把搜索摘要当作事实；
- 事后合理化失败结果。

因此，deep-research 在执行复杂任务时应尽量把上下文拆成可审计 artifact，而不是把所有过程连续塞进同一个 session。

### 1.2 Artifact first, chat second

重要信息必须沉淀为文件或结构化块：

- `research_contract.md`
- `literature_scout.md`
- `baseline_card.md`
- `idea_card.md`
- `evidence_map.json`
- `experiment_log.md`
- `result_review.md`

聊天上下文只用于调度，不作为唯一事实来源。

### 1.3 No fake research

禁止：

- 编造数据；
- 编造引用；
- 把搜索摘要当论文结论；
- 把未跑出的实验写成已验证；
- 根据结果反向修改成功标准；
- 用“看起来合理”的解释替代证据。

### 1.4 One task, one clean context

每个角色只接收完成任务所需的最小上下文。

例如 idea review 时，reviewer 只应看到：

- baseline 摘要；
- 当前一个 idea；
- research contract 中相关约束；
- 必要文献摘要。

不要让 reviewer 看到其它 9 个 idea、生成者的思考过程、之前被否掉的讨论和主 session 的聊天历史。

---

## 2. 推荐角色拆分

### 2.1 Main Architect

主 session，只负责：

- 定义任务边界；
- 生成或确认 research contract；
- 分派 scout / reviewer / writer；
- 读取 artifact 摘要；
- 做最终 go / revise / kill 判断。

主 session 不应该塞入大量搜索日志、安装日志、完整 PDF、训练日志。

### 2.2 Scout Agent

负责搜索和初筛。

输入：

- research contract；
- 研究问题；
- 时间范围；
- 推荐来源或白名单。

输出：

- 候选来源列表；
- 每个来源的摘要；
- 为什么相关；
- 是否有代码/数据/标准/专利；
- 证据强度；
- 不确定点。

### 2.3 Baseline Agent

用于深度学习科研或技术方案对比。

输出：

- baseline 为什么合适；
- 是否有开源代码；
- 数据集和评估指标；
- 复现难点；
- 可比性风险。

### 2.4 Idea Generator

只负责发散。

输入应尽量干净：

- 文献 scout 摘要；
- baseline card；
- research contract。

输出多个 idea，每个 idea 必须独立成卡片，不要混在长文里。

### 2.5 Independent Reviewer

每个 idea 单独评审。

输入：

- 当前 idea；
- baseline card；
- contract 中约束；
- 少量相关证据。

输出：

- novelty；
- feasibility；
- compatibility；
- expected upside；
- cost；
- risk；
- go / revise / kill。

### 2.6 Writer / Report Agent

只在证据和 contract 已确认后写报告。

写作阶段不能新增未经验证的 claim。

---

## 3. 标准流程

```text
0. Create research contract
1. Scout literature / sources
2. Build evidence map
3. Select baseline or comparison frame
4. Generate idea candidates or analysis hypotheses
5. Review ideas independently
6. Decide go / revise / kill
7. Run experiments or collect confirmatory evidence
8. Analyze result against contract
9. Write report or paper
10. QA: claim → evidence → source / result
```

---

## 4. 市场/技术调研版流程

适用于 `/research-public` 和 `/research-local`。

### Step 0 — Research Contract

先定义：

- 本次调研要回答什么；
- 哪些问题不回答；
- 允许使用哪些来源；
- 哪些资料不能联网；
- 关键结论需要什么证据等级；
- 最终输出给谁看。

### Step 1 — Source Scout

公开调研：优先使用权威来源、官网、标准、专利、论文、财报、公开新闻。

本地调研：只读用户指定目录，不联网。

### Step 2 — Evidence Map

每条关键结论必须映射到来源：

```text
claim_id → claim → source → evidence_strength → uncertainty
```

### Step 3 — Analysis Contract Check

写报告前检查：

- 是否有足够证据回答 contract；
- 是否存在信息缺口；
- 是否需要降级结论；
- 是否把观点和事实分开。

### Step 4 — Report Generation

报告中必须区分：

- confirmed fact；
- inferred judgment；
- assumption；
- uncertain / data gap。

---

## 5. 深度学习科研版流程

适用于 AI 辅助论文探索。

### Step 0 — Research Contract

在写代码前定义：

- hypothesis；
- baseline；
- dataset；
- metric；
- success signal；
- failure signal；
- ablation plan；
- compute budget；
- allowed implementation changes。

实验开始后 contract 不得被静默修改。如需修改，必须产出 v2，并记录原因。

### Step 1 — Literature Scout

只需要判断相关性时，不要把整篇 PDF 全塞进上下文。

优先读取：

- title；
- abstract；
- introduction；
- method overview；
- code availability。

能拿到 LaTeX / Markdown 源时优先使用源文件，而不是 PDF。

### Step 2 — Baseline Selection

大多数有效科研不是从零写代码，而是基于可靠 baseline 展开。

baseline card 至少包含：

- paper；
- official code；
- dataset；
- metric；
- expected reproduced score；
- environment requirements；
- known reproduction risk。

### Step 3 — Reproduction Hygiene

复现任务不要污染主 session：

- 用 tmux / job runner；
- 单独保存安装日志；
- 失败时只把 compact error summary 带回主 session；
- 数据集路径、GPU 文档、环境说明必须先读；
- 不允许无限 sleep；
- 下载后做 checksum 或文件指纹确认。

### Step 4 — Idea Generation and Review

生成多个 idea，但每个 idea 独立评审。

reviewer 不看其它 idea，不看生成过程。

每个 idea 必须落成 idea card。

### Step 5 — Result Review

评审结果时先读 contract，再看实验数据。

禁止结果出来后再修改成功标准。

写论文时，每条 claim 必须对应：

```text
contract signal → experiment result → figure/table/log → claim
```

---

## 6. 对 deep-research 的执行要求

1. 复杂任务先生成 research contract。
2. 公开资料和本地资料使用不同入口。
3. 不把敏感信息放进 `/research-public`。
4. 不把原始日志、长 PDF、安装报错灌入主上下文。
5. 每个阶段产出独立 artifact。
6. 最终报告必须能做 claim-level trace。
7. 对证据不足的地方主动降级表达。
8. 对不确定性和 data gap 单独列出。

---

## 7. 推荐文件命名

```text
reports/<topic>/<timestamp>/research_contract.md
reports/<topic>/<timestamp>/literature_scout.md
reports/<topic>/<timestamp>/baseline_card.md
reports/<topic>/<timestamp>/idea_cards/*.md
reports/<topic>/<timestamp>/evidence_map.json
reports/<topic>/<timestamp>/report.md
reports/<topic>/<timestamp>/qa.md
```

这能让后续重新开启干净 session 时，只读必要 artifact，不继承污染上下文。
