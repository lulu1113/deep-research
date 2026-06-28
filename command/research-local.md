---
description: 本地资料调研入口，只读取本地文件，不联网搜索，不抓取网页
---

<command-instruction>
Load and follow the `deep-research` skill exactly, but force offline local-file mode.

```text
skill(name="deep-research")
```

## Local-only boundary

This command is for internal documents, customer feedback, datasheets, PDF/DOCX/TXT/MD folders, and redacted project materials.

Hard requirements:

- Do not use websearch.
- Do not use SearXNG.
- Do not use Scrapling.
- Do not fetch external URLs.
- Read only the local files or directories provided by the user.
- If the path is missing, ask for the local path and stop.
- If the user asks to supplement online information, tell them to use `/research-public` after removing sensitive information.

## Parse mode

- `/research-local <path> <topic>` → standard mode
- `/research-local <path> <topic> -quick` → quick mode
- `/research-local <path> <topic> -deep` → deep mode

Pass the request to deep-research as local-file offline research, preserving the user's mode flag.
</command-instruction>

<user-request>
$ARGUMENTS
</user-request>
