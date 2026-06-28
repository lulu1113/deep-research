# deep-research Skill

<p align="center"><b>中文</b> · <a href="README_EN.md">English</a></p>

**深度调研报告生成 Skill — 一条命令，生成可引用、可浏览、可导出的深度调研报告。**

本 fork 基于 [hoolulu/deep-research](https://github.com/hoolulu/deep-research) 改造，保留原项目的多 Agent 调研流程、SearXNG 搜索、Scrapling 抓取、本地文件调研、报告质检和浏览器报告页能力，同时新增面向 **Hermes CLI / Codex CLI / Win11 原生环境 / 半导体研发市场调研** 的适配层。

> 默认外部 SearXNG 端点 `https://search.h33.top` 已保留；公开资料调研可继续使用。涉及客户、项目、内部型号、价格、路线图等敏感信息时，请使用本地离线入口。

---

## 本 fork 新增内容

| 文件/入口 | 作用 |
|---|---|
| `command/research-public.md` | 公开资料调研入口，允许联网搜索和 Scrapling 抓取，但禁止输入内部敏感信息 |
| `command/research-local.md` | 本地资料调研入口，强制离线，只读本地文件，不联网 |
| `command/update.md` | 受控更新入口，默认只检查差异，不直接 `git pull` |
| `docs/HERMES_INTEGRATION.md` | Hermes / 研发场景集成说明 |
| `docs/WINDOWS_HERMES_CODEX_INSTALL.md` | Win11 原生 Hermes CLI / Codex CLI 安装与使用指南 |
| `config/redaction_rules.md` | 客户名、项目名、内部型号、报价等脱敏规则 |
| `config/allowed_domains_semiconductor.txt` | 半导体/车规/LED/显示调研建议域名白名单 |
| `sources_semiconductor.json` | 半导体专用公开来源清单 |

---

## 推荐使用场景

### 适合 `/research-public` 的场景

用于公开资料调研，允许联网搜索、SearXNG、Scrapling 抓取。

适合：

- 行业趋势；
- 竞品公开资料；
- 官网、公开 datasheet、公开新闻；
- 标准、法规、政策；
- 公开专利；
- 半导体市场和技术路线研究。

示例：

```text
/research-public Local Dimming 分区背光驱动芯片竞争格局 -quick
/research-public 车载矩阵式大灯 LED Driver 技术路线 -standard
/research-public Automotive DCDC controller market landscape -deep
```

### 适合 `/research-local` 的场景

用于内部资料、本地文件、客户反馈、销售记录、未公开资料整理，强制离线，不联网。

适合：

- 客户需求整理；
- 销售反馈归纳；
- 本地 PDF/DOCX/TXT/MD 文件夹；
- 内部规格书对比；
- 未公开芯片规划；
- 会议纪要和项目资料。

示例：

```text
/research-local D:\资料库\LocalDimming 生成 Local Dimming 竞品对比报告 -quick
/research-local D:\客户反馈\2026Q2 整理 2026Q2 客户需求趋势 -standard
```

---

## 安全边界

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

如需处理这些资料，使用：

```text
/research-local <本地路径> <脱敏后的主题>
```

脱敏规则见：

```text
config/redaction_rules.md
```

---

## Win11 原生 Hermes CLI / Codex CLI 安装

详细步骤见：

```text
docs/WINDOWS_HERMES_CODEX_INSTALL.md
```

快速安装示例：

```powershell
$SkillBase = "$env:USERPROFILE\.agent-skills"
$Repo = "$SkillBase\deep-research"

New-Item -ItemType Directory -Force $SkillBase | Out-Null

git clone https://github.com/lulu1113/deep-research.git $Repo
cd $Repo

py -3 -m venv .venv
& "$Repo\.venv\Scripts\python.exe" -m pip install -U pip
& "$Repo\.venv\Scripts\pip.exe" install scrapling mcp pypdf2 python-docx
```

Hermes 目录联接：

```powershell
$HermesSkills = "$env:USERPROFILE\.hermes\skills"
New-Item -ItemType Directory -Force $HermesSkills | Out-Null
cmd /c mklink /J "%USERPROFILE%\.hermes\skills\deep-research" "%USERPROFILE%\.agent-skills\deep-research"
```

Codex 目录联接：

```powershell
$CodexSkills = "$env:USERPROFILE\.codex\skills"
New-Item -ItemType Directory -Force $CodexSkills | Out-Null
cmd /c mklink /J "%USERPROFILE%\.codex\skills\deep-research" "%USERPROFILE%\.agent-skills\deep-research"
```

如果 Hermes / Codex 不自动扫描 `command/` 目录，可手动复制命令入口：

```powershell
$HermesCommands = "$env:USERPROFILE\.hermes\commands"
New-Item -ItemType Directory -Force $HermesCommands | Out-Null
Copy-Item "$Repo\command\research-public.md" "$HermesCommands\research-public.md" -Force
Copy-Item "$Repo\command\research-local.md" "$HermesCommands\research-local.md" -Force
Copy-Item "$Repo\command\update.md" "$HermesCommands\research-update.md" -Force

$CodexCommands = "$env:USERPROFILE\.codex\commands"
New-Item -ItemType Directory -Force $CodexCommands | Out-Null
Copy-Item "$Repo\command\research-public.md" "$CodexCommands\research-public.md" -Force
Copy-Item "$Repo\command\research-local.md" "$CodexCommands\research-local.md" -Force
Copy-Item "$Repo\command\update.md" "$CodexCommands\research-update.md" -Force
```

---

## Scrapling MCP 配置

### Hermes `config.json` 示例

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

### Codex `config.toml` 示例

```toml
[mcp_servers.scrapling]
command = "C:\\Users\\<用户名>\\.agent-skills\\deep-research\\.venv\\Scripts\\python.exe"
args = [
  "C:\\Users\\<用户名>\\.agent-skills\\deep-research\\scrapling-mcp-server.py"
]
```

Scrapling 只建议配合 `/research-public` 使用；`/research-local` 不应调用 websearch、SearXNG、Scrapling 或任何外部 URL。

---

## 原项目核心能力

原 deep-research Skill 的核心流程仍然保留：

```text
① 分析大纲 — 分析主题，生成调研框架和搜索计划
② 采集数据 — 在线模式五层搜索 / 离线模式读取本地文件
③ 并行撰写 — 多章节并行写作
④ 验收装配 — validate → assemble-report → convert-citations → qa-report
```

三种深度模式：

| 命令 | 用途 | 最少章数 | 最少段落/章 | 参考字数 |
|---|---|---:|---:|---:|
| `/research 主题` | standard 默认 | 8 | ≥ 5 | ≈ 25,000 |
| `/research 主题 -quick` | 快速洞察 | 5 | ≥ 4 | ≈ 15,000 |
| `/research 主题 -deep` | 极致深度 | 10 | ≥ 6 | ≈ 45,000 |

本 fork 推荐在 Hermes/Codex 中优先使用：

| 命令 | 用途 | 是否联网 |
|---|---|---:|
| `/research-public <主题>` | 公开资料 standard 调研 | 是 |
| `/research-public <主题> -quick` | 公开资料快速调研 | 是 |
| `/research-public <主题> -deep` | 公开资料深度调研 | 是 |
| `/research-local <路径> <主题>` | 本地资料 standard 调研 | 否 |
| `/research-local <路径> <主题> -quick` | 本地资料快速调研 | 否 |
| `/research-local <路径> <主题> -deep` | 本地资料深度调研 | 否 |

---

## 半导体/车规/显示/LED 调研增强

本 fork 新增：

```text
config/allowed_domains_semiconductor.txt
sources_semiconductor.json
```

建议优先用于：

- Local Dimming 分区背光驱动芯片；
- 汽车大灯/尾灯/氛围灯 LED Driver；
- 汽车 DCDC 控制器和转换器；
- 车规认证、AEC-Q、JEDEC、ISO、SAE；
- OSRAM、TI、ADI、MPS、Infineon、ST、NXP、onsemi、Renesas 等公开资料调研。

---

## 更新方式

更新本 fork：

```powershell
cd $env:USERPROFILE\.agent-skills\deep-research
git pull origin main
```

不建议直接自动同步上游。`command/update.md` 已改为受控更新：默认只检查差异，不直接 `git pull`。只有用户明确确认时才允许更新。

如需审查上游：

```powershell
git remote add upstream https://github.com/hoolulu/deep-research.git
git fetch upstream
git diff --stat main..upstream/main
git log --oneline main..upstream/main
```

确认不会覆盖 Hermes 适配、安全规则、半导体源清单和脱敏配置后，再人工合并。

---

## 相关文档

- [Hermes / 研发场景集成说明](docs/HERMES_INTEGRATION.md)
- [Win11 原生 Hermes CLI / Codex CLI 安装与使用指南](docs/WINDOWS_HERMES_CODEX_INSTALL.md)
- [脱敏规则](config/redaction_rules.md)
- [半导体域名白名单](config/allowed_domains_semiconductor.txt)
- [半导体 sources 配置](sources_semiconductor.json)

---

## License

MIT

本 fork 继承上游项目 MIT License。原项目作者：[hoolulu](https://github.com/hoolulu)，上游项目地址：[github.com/hoolulu/deep-research](https://github.com/hoolulu/deep-research)。
