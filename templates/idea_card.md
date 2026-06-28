# Idea Card Template

> 每个 idea 必须独立评审。不要把多个 idea 混在一张卡里，不要让 reviewer 看到其它 idea 或生成过程。

## 0. Metadata

- idea_id:
- created_at:
- source: human / model / literature / experiment observation
- related_contract:
- related_baseline:
- review_status: pending / go / revise / kill

## 1. One-line Summary

```text

```

## 2. Motivation

这个 idea 想解决什么问题？

- 

## 3. Method Sketch

核心方法是什么？

- 

## 4. Expected Mechanism

为什么它可能有效？

- 

## 5. Novelty Check

| Question | Answer |
|---|---|
| 是否已有相似工作？ |  |
| 与 baseline 的差异是什么？ |  |
| 与最近论文的差异是什么？ |  |
| novelty 风险 | low / medium / high |

## 6. Feasibility Check

| Dimension | Score 1-5 | Notes |
|---|---:|---|
| implementation complexity |  |  |
| compute cost |  |  |
| data requirement |  |  |
| baseline compatibility |  |  |
| debugging risk |  |  |

## 7. Expected Upside

- main metric:
- expected improvement:
- secondary benefits:
- what would be surprising:

## 8. Success / Failure Signals

### Success signals

- 

### Failure signals

- 

### Inconclusive signals

- 

## 9. Required Experiments or Evidence

| Experiment / Evidence | Purpose | Expected signal | Artifact |
|---|---|---|---|
|  |  |  |  |

## 10. Independent Review

Reviewer 只能基于当前 idea、contract、baseline card 和必要证据评审。

| Dimension | Score 1-5 | Reason |
|---|---:|---|
| novelty |  |  |
| technical feasibility |  |  |
| compatibility |  |  |
| expected upside |  |  |
| cost control |  |  |
| risk |  |  |

Final decision: go / revise / kill

Reason:

```text

```

## 11. If Revise

必须修改什么？

- 

修改后是否需要重新 review：yes / no

## 12. If Kill

kill 原因：

- not novel
- not feasible
- incompatible with baseline
- too costly
- weak expected upside
- evidence contradicts it
- other:

## 13. Traceability

如果最终进入报告或论文，每条 claim 必须能追溯到：

```text
idea → contract signal → experiment/evidence → result → claim
```
