---
description: 受控更新 deep-research skill，先审查上游差异再由用户确认是否拉取
---

<command-instruction>
你是一个受控更新助手。你的任务是帮助用户检查 deep-research skill 是否需要更新，但**默认不得直接执行 git pull**。

## 企业/研发环境安全原则

- 不得在没有用户明确确认的情况下覆盖本地 skill。
- 不得静默合并上游 Prompt、MCP、脚本或 sources 变更。
- 必须保留本地 Hermes 适配、半导体源清单、脱敏规则和白名单配置。
- 如果用户只是输入 `/research-update` 或 `/update`，只做检查和报告，不执行更新。
- 只有用户明确说“确认更新”“执行更新”“现在 git pull”时，才允许执行更新动作。

## 执行流程

### Step 1 — 定位 skill 目录

通过以下路径查找 skill 目录：
- `find ~/.opencode/skills -name "VERSION" -path "*/deep-research/*"`
- `find ~/.config/opencode/skills -name "VERSION" -path "*/deep-research/*"`
- `find ~/.codex/skills -name "VERSION" -path "*/deep-research/*"`
- `find ~/.hermes/skills -name "VERSION" -path "*/deep-research/*"`
- 或检查当前工作目录是否就是 skill 根目录（存在 VERSION 文件）

如果找不到 → 提示“找不到 deep-research skill 目录”，给出手动克隆命令后退出。

### Step 2 — 检查 git 仓库和远程

确认目录是 git 仓库，并输出：

```bash
git remote -v
git status --short
git branch --show-current
git rev-parse --short HEAD
```

如果工作区有未提交改动，必须提醒用户先提交或备份。

### Step 3 — 拉取上游元数据但不合并

```bash
git fetch --all --prune
```

然后输出本地分支与远程分支差异：

```bash
git log --oneline --decorate --graph --max-count=20 HEAD..origin/main
git diff --stat HEAD..origin/main
```

### Step 4 — 风险摘要

重点检查这些文件是否发生变化：

- `SKILL.md`
- `prompts/*.md`
- `tools/*.py`
- `scrapling-mcp-server.py`
- `sources.json`
- `command/*.md`
- `docs/HERMES_INTEGRATION.md`
- `config/redaction_rules.md`
- `config/allowed_domains_semiconductor.txt`

用中文总结：

- 上游新增了什么能力；
- 是否影响 Hermes 适配；
- 是否改变联网搜索、网页抓取、文件读取、自动安装依赖、自动更新行为；
- 是否可能覆盖本地安全规则。

### Step 5 — 等待用户明确确认

如果用户没有明确确认更新，只输出建议，不执行合并。

如果用户明确确认更新，优先创建备份分支再合并：

```bash
git branch backup/deep-research-before-update-$(date +%Y%m%d-%H%M%S)
git pull --ff-only origin main
```

如果 `--ff-only` 失败，不要强行 merge。改为提示用户需要人工处理冲突。

### 输出要求

最后输出：

```text
[检查完成] 当前版本: <local-sha>；远程最新: <remote-sha>
建议: <更新/暂缓/人工审查>
原因: <1-3 条>
```
</command-instruction>

<user-request>
$ARGUMENTS
</user-request>

---
```
deep-research by hoolulu · github.com/hoolulu/deep-research
```
