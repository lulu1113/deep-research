# Hermes / 研发场景集成说明

本文件用于把 `deep-research` 改造成适合 Hermes、Codex CLI 和研发/市场调研场景的受控 Skill。

## 结论

建议把本仓库作为 Hermes 的“公开资料调研 + 本地资料整理”Skill 使用，而不是作为可信事实数据库或机密资料自动联网分析器。

## 默认外部 SearXNG 端点

默认外部 SearXNG 端点保留：

```text
https://search.h33.top
```

这是上游项目的默认搜索补充端点。公开资料调研可以继续使用它；涉及内部资料、客户项目、未发布产品、价格、路线图时，不要使用联网入口。

如后续需要企业内网化，可以再把搜索端点迁移到自建 SearXNG，但本适配不删除默认端点。

## 推荐命令入口

### 1. `/research-public`

用于公开资料调研，允许联网搜索和网页抓取。

适合：

- 行业趋势；
- 竞品公开资料；
- 政策/标准；
- 公开技术路线；
- 半导体市场报告。

示例：

```text
/research-public Local Dimming 分区背光驱动芯片竞争格局 -quick
/research-public 车载矩阵式大灯 LED Driver 技术路线 -standard
/research-public Automotive DCDC controller market landscape -deep
```

### 2. `/research-local`

用于内部资料、本地 PDF/DOCX/TXT/MD 文件夹调研，强制离线，不联网。

适合：

- 客户需求整理；
- 销售反馈归纳；
- 内部规格书对比；
- 本地竞品资料包；
- 会议纪要和项目资料。

示例：

```text
/research-local D:\资料库\LocalDimming 生成 Local Dimming 竞品对比报告 -quick
/research-local D:\客户反馈\2026Q2 整理 2026Q2 客户需求趋势 -standard
```

## Hermes 集成步骤

1. 将本仓库放入 Hermes 的 skills 或 tools 目录。
2. 注册 `command/research-public.md` 和 `command/research-local.md` 为 Hermes 命令入口。
3. 注册 Scrapling MCP Server，但仅允许 `/research-public` 使用。
4. 对联网模式加入 `config/allowed_domains_semiconductor.txt` 作为建议白名单。
5. 对内部资料先应用 `config/redaction_rules.md` 中的脱敏规则。
6. 生成报告后，再由独立流程决定是否入库 SQLite / 飞书多维表格。

## 安全边界

| 场景 | 使用入口 | 是否联网 |
|---|---|---:|
| 公开行业调研 | `/research-public` | 是 |
| 竞品官网资料 | `/research-public` | 是 |
| 客户项目资料 | `/research-local` | 否 |
| 内部芯片型号规划 | `/research-local` | 否 |
| 销售反馈/邮件/微信整理 | `/research-local` | 否 |
| 脱敏后的公开趋势分析 | `/research-public` | 是 |

## 上游更新策略

`command/update.md` 已改为受控更新：默认只检查差异，不直接 `git pull`。

推荐流程：

```bash
git fetch --all --prune
git diff --stat HEAD..origin/main
git log --oneline HEAD..origin/main
```

确认上游没有覆盖 Hermes 适配、安全规则和本地配置后，再手动合并。

## 后续可扩展方向

- 把 `data-pool.json` 导出到 SQLite；
- 把客户需求字段同步到飞书多维表格；
- 增加半导体专用报告模板；
- 增加竞品 datasheet 参数抽取器；
- 增加本地 RAG 索引，避免每次从头读文件。
