---
description: 深度调研任何主题，生成对标券商/研究机构标准的专业报告
---

<command-instruction>
Load and follow the `deep-research` skill exactly.

```text
skill(name="deep-research")
```

Parse `$ARGUMENTS` to determine the research topic and optional mode flags:
- `/research 主题` → standard 模式
- `/research 主题 -quick` → quick 模式
- `/research 主题 -deep` → deep 模式
</command-instruction>

<user-request>
$ARGUMENTS
</user-request>
