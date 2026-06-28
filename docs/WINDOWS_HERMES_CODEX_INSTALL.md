# Win11 原生 Hermes CLI / Codex CLI 安装与使用指南

本文档说明如何在 **Windows 11 原生环境** 中，把本仓库安装为 Hermes CLI 和 Codex CLI 可复用的 deep-research Skill。

适用环境：

- Windows 11；
- PowerShell 7；
- Git for Windows；
- Python 3；
- Hermes CLI；
- Codex CLI；
- 不使用 WSL 路径。

本 fork 新增了 context-first 工作流，推荐先用 `/research-contract` 锁定调研范围、证据标准和上下文计划，再执行 `/research-public` 或 `/research-local`。

---

## 1. 设计原则

建议使用一份统一的 Skill 仓库，然后通过 Windows 目录联接挂载到 Hermes 和 Codex：

```text
C:\Users\<用户名>\.agent-skills\deep-research
C:\Users\<用户名>\.hermes\skills\deep-research  -> 指向 .agent-skills\deep-research
C:\Users\<用户名>\.codex\skills\deep-research   -> 指向 .agent-skills\deep-research
```

这样做的好处：

- 只维护一份代码；
- Hermes CLI 和 Codex CLI 共用同一份 Skill；
- 后续 `git pull origin main` 一次即可更新；
- 不会因为两个目录内容不一致导致行为差异。

---

## 2. 安装到公共 Skill 目录

打开 PowerShell 7，执行：

```powershell
$SkillBase = "$env:USERPROFILE\.agent-skills"
$Repo = "$SkillBase\deep-research"

New-Item -ItemType Directory -Force $SkillBase | Out-Null

git clone https://github.com/lulu1113/deep-research.git $Repo
cd $Repo
```

如果之前已经 clone 过，则执行：

```powershell
$SkillBase = "$env:USERPROFILE\.agent-skills"
$Repo = "$SkillBase\deep-research"

cd $Repo
git pull origin main
```

---

## 3. 安装 Python 依赖

在仓库目录中创建虚拟环境：

```powershell
cd $Repo

py -3 -m venv .venv
& "$Repo\.venv\Scripts\python.exe" -m pip install -U pip
& "$Repo\.venv\Scripts\pip.exe" install scrapling mcp pypdf2 python-docx
```

测试默认外部 SearXNG 检测：

```powershell
& "$Repo\.venv\Scripts\python.exe" tools\dr_tools.py detect-engine
```

本仓库保留上游默认外部 SearXNG 端点：

```text
https://search.h33.top
```

它只建议用于公开资料调研。内部客户资料、项目资料、未发布型号、价格和路线图不要使用联网模式。

如果后续需要抓取 JavaScript 页面，可补装 Playwright：

```powershell
& "$Repo\.venv\Scripts\pip.exe" install playwright
& "$Repo\.venv\Scripts\python.exe" -m playwright install chromium
```

---

## 4. 安装到 Hermes CLI

### 4.1 创建 Hermes skills 目录

```powershell
$HermesSkills = "$env:USERPROFILE\.hermes\skills"
New-Item -ItemType Directory -Force $HermesSkills | Out-Null
```

### 4.2 创建目录联接

```powershell
cmd /c mklink /J "%USERPROFILE%\.hermes\skills\deep-research" "%USERPROFILE%\.agent-skills\deep-research"
```

如果提示目录已存在，先删除旧目录或旧链接：

```powershell
Remove-Item "$env:USERPROFILE\.hermes\skills\deep-research" -Recurse -Force
cmd /c mklink /J "%USERPROFILE%\.hermes\skills\deep-research" "%USERPROFILE%\.agent-skills\deep-research"
```

### 4.3 注册 Hermes 命令入口

如果 Hermes 会自动扫描 skill 目录里的 `command/` 文件夹，可以跳过本步骤。

如果 Hermes 使用独立 commands 目录，执行：

```powershell
$HermesCommands = "$env:USERPROFILE\.hermes\commands"
New-Item -ItemType Directory -Force $HermesCommands | Out-Null

Copy-Item "$Repo\command\research-contract.md" "$HermesCommands\research-contract.md" -Force
Copy-Item "$Repo\command\research-public.md" "$HermesCommands\research-public.md" -Force
Copy-Item "$Repo\command\research-local.md" "$HermesCommands\research-local.md" -Force
Copy-Item "$Repo\command\update.md" "$HermesCommands\research-update.md" -Force
```

---

## 5. 给 Hermes 配置 Scrapling MCP

Hermes 配置文件常见路径：

```text
C:\Users\<用户名>\.hermes\config.json
```

可以加入类似配置：

```json
{
  "mcpServers": {
    "scrapling": {
      "command": "C:\\Users\\<用户名>\\.agent-skills\\deep-research\\.venv\\Scripts\\python.exe",
      "args": [
        "C:\\Users\\<用户名>\\.agent-skills\\deep-research\\scrapling-mcp-server.py"
      ]
    }
  }
}
```

将 `<用户名>` 替换为真实 Windows 用户名。可以用下面命令查看：

```powershell
$env:USERNAME
```

注意：Scrapling 只建议给 `/research-public` 使用。内部资料、客户资料、销售反馈和未公开研发资料应使用 `/research-local`。

---

## 6. 安装到 Codex CLI

### 6.1 创建 Codex skills 目录

```powershell
$CodexSkills = "$env:USERPROFILE\.codex\skills"
New-Item -ItemType Directory -Force $CodexSkills | Out-Null
```

### 6.2 创建目录联接

```powershell
cmd /c mklink /J "%USERPROFILE%\.codex\skills\deep-research" "%USERPROFILE%\.agent-skills\deep-research"
```

如果目录已存在：

```powershell
Remove-Item "$env:USERPROFILE\.codex\skills\deep-research" -Recurse -Force
cmd /c mklink /J "%USERPROFILE%\.codex\skills\deep-research" "%USERPROFILE%\.agent-skills\deep-research"
```

### 6.3 注册 Codex 命令入口

如果 Codex CLI 会自动扫描 skill 目录中的 `command/` 文件夹，可以跳过本步骤。

如果 Codex CLI 使用独立 commands 目录，执行：

```powershell
$CodexCommands = "$env:USERPROFILE\.codex\commands"
New-Item -ItemType Directory -Force $CodexCommands | Out-Null

Copy-Item "$Repo\command\research-contract.md" "$CodexCommands\research-contract.md" -Force
Copy-Item "$Repo\command\research-public.md" "$CodexCommands\research-public.md" -Force
Copy-Item "$Repo\command\research-local.md" "$CodexCommands\research-local.md" -Force
Copy-Item "$Repo\command\update.md" "$CodexCommands\research-update.md" -Force
```

如果当前 Codex CLI 版本不识别 slash command，可以显式要求 Codex 读取命令文件：

```text
请读取 C:\Users\<用户名>\.agent-skills\deep-research\command\research-contract.md，
按其中规则执行：
为 Local Dimming 分区背光驱动芯片竞争格局调研生成 research contract
```

公开资料模式：

```text
请读取 C:\Users\<用户名>\.agent-skills\deep-research\command\research-public.md，
按其中规则执行：
Local Dimming 分区背光驱动芯片竞争格局 -quick
```

本地资料模式：

```text
请读取 C:\Users\<用户名>\.agent-skills\deep-research\command\research-local.md，
按其中规则执行：
D:\资料库\LocalDimming 生成竞品对比报告 -quick
```

---

## 7. 给 Codex CLI 配置 Scrapling MCP

Codex CLI 配置文件常见路径：

```text
C:\Users\<用户名>\.codex\config.toml
```

可以加入：

```toml
[mcp_servers.scrapling]
command = "C:\\Users\\<用户名>\\.agent-skills\\deep-research\\.venv\\Scripts\\python.exe"
args = [
  "C:\\Users\\<用户名>\\.agent-skills\\deep-research\\scrapling-mcp-server.py"
]
```

将 `<用户名>` 替换为真实 Windows 用户名。

---

## 8. 测试 Hermes CLI

启动 Hermes CLI：

```powershell
hermes
```

先测试 contract：

```text
/research-contract Local Dimming 分区背光驱动芯片竞争格局，面向产品规划会议，要求区分事实、推断和数据缺口
```

测试公开资料联网调研：

```text
/research-public Local Dimming 分区背光驱动芯片竞争格局 -quick
```

测试本地资料离线调研：

```text
/research-local D:\资料库\LocalDimming 生成 Local Dimming 竞品对比报告 -quick
```

---

## 9. 测试 Codex CLI

启动 Codex CLI：

```powershell
codex
```

先测试 contract：

```text
/research-contract 车载矩阵式大灯 LED Driver 技术路线，要求输出可验证的公开来源和 data gap
```

测试公开资料联网调研：

```text
/research-public 车载矩阵式大灯 LED Driver 技术路线 -standard
```

如果 Codex 不识别 slash command，使用显式命令文件方式：

```text
请使用 C:\Users\<用户名>\.agent-skills\deep-research\command\research-public.md 这个命令规则，
调研：车载矩阵式大灯 LED Driver 技术路线 -standard
```

---

## 10. 日常使用建议

### 10.1 复杂任务先立约

```text
/research-contract Local Dimming 分区背光驱动芯片竞争格局，输出给产品规划会议使用
```

### 10.2 公开资料调研

适合行业趋势、竞品官网、公开政策、公开标准、公开专利、公开技术路线。

```text
/research-public Local Dimming 分区背光驱动芯片竞争格局 -quick
/research-public 车载矩阵式大灯 LED Driver 技术路线 -standard
/research-public Automotive DCDC controller market landscape -deep
```

### 10.3 本地资料调研

适合内部资料、客户反馈、销售记录、本地 PDF/DOCX/TXT/MD 文件夹、未公开资料。

```text
/research-local D:\客户反馈\2026Q2 整理 2026Q2 客户需求趋势 -standard
/research-local D:\资料库\LocalDimming 生成竞品对比报告 -quick
```

### 10.4 敏感资料边界

以下内容不要进入 `/research-public`：

- 客户名称；
- 项目代号；
- 内部芯片型号；
- 未发布产品规划；
- 报价、毛利、成本；
- 合同条款；
- 供应商未公开信息；
- 量产时间表；
- 客户邮件、微信、联系人信息。

如需处理这些资料，使用 `/research-local`，并优先参考：

```text
config/redaction_rules.md
```

---

## 11. Context-first 使用原则

详细规则见：

```text
config/context_policy.md
docs/CONTEXT_FIRST_RESEARCH_WORKFLOW.md
templates/research_contract.md
templates/idea_card.md
templates/baseline_card.md
```

关键点：

- 主 session 只保留调度信息和 artifact 摘要；
- 不把搜索日志、安装日志、长 PDF、训练日志塞进主上下文；
- 每个 idea 独立评审；
- 每个关键 claim 必须能追溯到 evidence；
- contract 开始执行后不能静默修改。

---

## 12. 更新方式

更新你的 fork：

```powershell
cd $env:USERPROFILE\.agent-skills\deep-research
git pull origin main
```

不建议直接自动同步上游仓库。当前 `command/update.md` 已改成受控更新：默认只检查差异，不直接 `git pull`，只有用户明确确认才允许更新。

如需审查上游：

```powershell
cd $env:USERPROFILE\.agent-skills\deep-research
git remote add upstream https://github.com/hoolulu/deep-research.git
git fetch upstream
git diff --stat main..upstream/main
git log --oneline main..upstream/main
```

确认不会覆盖 Hermes 适配、安全规则、context policy、半导体源清单和脱敏配置后，再人工合并。

---

## 13. 常见问题

### 13.1 `mklink` 失败

优先使用 PowerShell 管理员权限打开，或启用 Windows 开发者模式。也可以不用链接，直接复制目录到 `.hermes\skills` 和 `.codex\skills`，但后续需要分别更新。

### 13.2 `py -3` 找不到

检查 Python 是否安装，并确认加入 PATH：

```powershell
python --version
py --version
```

如果没有 `py`，可改用：

```powershell
python -m venv .venv
```

### 13.3 pip 下载失败

如果需要走代理，先在当前 PowerShell 设置代理：

```powershell
$env:HTTP_PROXY="http://127.0.0.1:7890"
$env:HTTPS_PROXY="http://127.0.0.1:7890"
```

然后重新执行 pip 安装。

### 13.4 Codex 不识别 slash command

使用显式命令文件方式，让 Codex 读取：

```text
C:\Users\<用户名>\.agent-skills\deep-research\command\research-contract.md
C:\Users\<用户名>\.agent-skills\deep-research\command\research-public.md
C:\Users\<用户名>\.agent-skills\deep-research\command\research-local.md
```

### 13.5 公开模式与本地模式怎么选

- 先定义范围和证据标准：`/research-contract`。
- 公开资料、行业趋势、竞品官网：`/research-public`。
- 内部资料、客户反馈、销售记录、本地文件夹：`/research-local`。
- 不确定是否敏感时，先用 `/research-local`。
