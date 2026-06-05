---
description: 检查 deep-research skill 是否有新版本可用，并执行更新
---

<command-instruction>
你是一个版本检查工具。任务是检查 deep-research skill 的本地版本是否落后于远程版本，如有更新则执行拉取。

## 检查流程

### Step 1 — 读取本地版本
读取本地 `VERSION` 文件获取当前版本号：
- skill 目录可以通过 find 命令定位：`find ~/.opencode/skills -name "VERSION" -path "*/deep-research/*"` 或 `find ~/.config/opencode/skills -name "VERSION" -path "*/deep-research/*"`
- 读取文件内容即为版本号，例如 `1.0.0`

### Step 2 — 获取远程版本
用 `webfetch` 获取远程 VERSION 文件：
`webfetch(url="https://raw.githubusercontent.com/hoolulu/deep-research/main/VERSION")`
提取内容中的版本号。

### Step 3 — 比较版本
- 如果本地版本 < 远程版本 → 提示有更新，询问用户是否要更新
- 如果本地版本 >= 远程版本 → 提示已是最新
- 如果无法获取远程版本 → 提示网络问题，跳过

### Step 4 — 执行更新（用户确认后）
先检查 skill 目录是否是通过 git clone 安装的（存在 `.git` 目录）：
- 有 `.git` → 在 skill 目录执行 `git pull`
- 无 `.git` → 提示手动更新方式：前往 https://github.com/hoolulu/deep-research 下载最新版，替换 skill 目录

### 输出示例

```
当前版本：1.0.0
远程版本：1.1.0

📦 有新版本可用！更新内容：
- 修复目录生成规则
- 优化数据池结构

是否执行更新？[y/N]
```

更新完成后：
```
[✓] 已更新到 v1.1.0
更新内容：请查看 CHANGELOG 或 git log
```
</command-instruction>

<user-request>
$ARGUMENTS
</user-request>
