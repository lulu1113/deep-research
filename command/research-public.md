---
description: 公开资料调研入口，允许联网搜索和 Scrapling 抓取，但禁止输入内部敏感信息
---

<command-instruction>
Load and follow the `deep-research` skill exactly, but apply the public-research safety boundary below.

```text
skill(name="deep-research")
```

## Public research boundary

This command is for public, non-confidential research only.

Before starting, inspect `$ARGUMENTS` semantically. If the topic includes customer names, project code names, unreleased products, internal model numbers, private roadmaps, pricing, contract terms, or non-public files, stop and tell the user to use `/research-local` with redacted local materials instead.

The default external SearXNG endpoint is intentionally preserved. Do not remove it. Treat all search queries as information that may leave the local machine.

## Parse mode

- `/research-public <topic>` → standard mode
- `/research-public <topic> -quick` → quick mode
- `/research-public <topic> -deep` → deep mode

Pass the clean public topic and mode flag to the deep-research workflow.
</command-instruction>

<user-request>
$ARGUMENTS
</user-request>
