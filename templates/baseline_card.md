# Baseline Card Template

> 大多数有效研究不是从零开始，而是基于可靠 baseline 展开。Baseline card 用于确保后续 idea、实验和报告有可比较的锚点。

## 0. Metadata

- baseline_id:
- created_at:
- related_contract:
- owner:

## 1. Baseline Identity

- name:
- paper / source:
- official code:
- license:
- domain:
- task / setting:

## 2. Why This Baseline

为什么选择它作为锚点？

- 

## 3. Reproduction Target

| Item | Value |
|---|---|
| dataset |  |
| split |  |
| metric |  |
| expected score |  |
| hardware |  |
| training time |  |
| environment |  |

## 4. Available Artifacts

- paper:
- code:
- pretrained weights:
- dataset path:
- config path:
- logs:

## 5. Reproduction Risks

| Risk | Severity | Mitigation |
|---|---|---|
| dependency conflict | low / medium / high |  |
| dataset unavailable | low / medium / high |  |
| undocumented preprocessing | low / medium / high |  |
| random seed instability | low / medium / high |  |
| hardware mismatch | low / medium / high |  |

## 6. Context Hygiene

复现时不要污染主 session。

- 原始安装日志保存到文件；
- 下载日志保存到文件；
- 训练日志保存到文件；
- 主 session 只接收 compact summary；
- 失败时返回 command、exit code、last relevant error、suspected cause、next action。

## 7. Comparison Rules

后续 idea 必须和 baseline 在同一 setting 下比较：

- same dataset;
- same split;
- same metric;
- same or clearly reported compute budget;
- same preprocessing unless contract explicitly允许修改。

## 8. Reproduction Result

| Run | Config | Score | Status | Log path |
|---|---|---:|---|---|
|  |  |  |  |  |

## 9. Decision

Baseline status: ready / partially ready / failed / replaced

Reason:

```text

```
