# Research Contract Template

> 在任何复杂调研、实验、竞品分析或论文探索开始前填写。Contract 是后续判断的唯一尺子，开始执行后不得静默修改。

## 0. Metadata

- contract_id:
- version: v1
- created_at:
- owner:
- mode: public / local / experiment
- target_output: report / memo / paper / decision brief / other

## 1. Research Question

本次任务要回答的核心问题：

```text

```

## 2. Scope

### In scope

- 

### Out of scope

- 

## 3. Confidentiality Boundary

是否包含敏感信息：yes / no

如 yes，只能使用 `/research-local`。

敏感项包括：

- 客户名称；
- 项目代号；
- 内部芯片型号；
- 未发布产品规划；
- 报价、毛利、成本；
- 合同条款；
- 供应商未公开信息；
- 客户邮件、微信、联系人信息。

## 4. Allowed Sources

### Public mode

允许来源：

- 官网；
- datasheet；
- 标准/法规；
- 公开论文；
- 公开专利；
- 公开财报；
- 权威行业媒体；
- 其它：

### Local mode

允许本地路径：

```text

```

禁止联网：yes / no

## 5. Baseline / Comparison Frame

适用于技术对比、市场对比、深度学习科研。

- baseline / comparison target:
- 为什么选它：
- 公开代码 / 数据 / 文档：
- 预期可比指标：
- 可比性风险：

## 6. Hypothesis / Expected Findings

在看到结果前写清楚预期。

### Hypothesis

- 

### Success signals

什么结果算成功：

- 

### Failure signals

什么结果算失败，不能只写“没达到成功”：

- 

### Neutral / inconclusive signals

什么结果只能说明证据不足：

- 

## 7. Metrics and Evidence Standard

| Claim type | Required evidence | Minimum confidence |
|---|---|---|
| confirmed fact | primary source / official doc / reproducible result | high |
| market estimate | at least 2 independent sources or clearly marked estimate | medium |
| technical judgment | source + reasoning + uncertainty | medium |
| speculation | clearly labeled assumption | low |

## 8. Experiments / Checks / Ablations

适用于深度学习科研或需要验证的技术分析。

| Check | Purpose | Expected signal | Failure signal | Artifact |
|---|---|---|---|---|
|  |  |  |  |  |

## 9. Claim Traceability Requirement

最终报告每条关键结论必须能映射到：

```text
claim → source / file / result → evidence strength → confidence
```

不满足该映射的内容只能写为假设、推断或 data gap。

## 10. Context Plan

主 session 只保留：

- contract；
- artifact 摘要；
- 最终决策。

需要单独保存的 artifact：

- source_scout.md
- evidence_map.json
- baseline_card.md
- idea_cards/*.md
- result_review.md
- final_report.md
- qa.md

## 11. Revision Policy

Contract 创建后不得静默修改。

如需修改，创建 `research_contract_v2.md`，并写明：

- 修改内容；
- 修改原因；
- 修改发生在看到哪些结果之前或之后；
- 对结论的影响。
