---
description: 公开资料调研入口，允许联网搜索和 Scrapling 抓取，但禁止输入内部敏感信息
---

<command-instruction>
Load and follow the `deep-research` skill exactly, but apply the public-research safety boundary and context-first workflow below.

```text
skill(name="deep-research")
```

Also follow these repository rules conceptually:

```text
config/context_policy.md
templates/research_contract.md
docs/CONTEXT_FIRST_RESEARCH_WORKFLOW.md
```

## Public research boundary

This command is for public, non-confidential research only.

Before starting, inspect `$ARGUMENTS` semantically. If the topic includes customer names, project code names, unreleased products, internal model numbers, private roadmaps, pricing, contract terms, non-public files, or private communications, stop and tell the user to use `/research-local` with redacted local materials instead.

The default external SearXNG endpoint is intentionally preserved. Do not remove it. Treat all search queries as information that may leave the local machine.

## Context-first requirements

- Do not let the main session become a dump of search logs, raw webpages, failed attempts, or long PDF text.
- For complex research, first generate or infer a compact research contract: question, scope, evidence standard, output type, and out-of-scope boundaries.
- Keep source scouting, evidence extraction, analysis, and writing as separate stages.
- Before writing the final report, build an evidence map: `claim → source → evidence strength → confidence`.
- Mark every key conclusion as confirmed fact, inferred judgment, assumption, or data gap.
- Do not use search snippets as final evidence when full text can be fetched.
- Do not overstate conclusions if evidence is weak or contradictory.

## Parse mode

- `/research-public <topic>` → standard mode
- `/research-public <topic> -quick` → quick mode
- `/research-public <topic> -deep` → deep mode

Pass the clean public topic and mode flag to the deep-research workflow.

## Suggested pre-step

For high-stakes, long, or ambiguous topics, recommend running:

```text
/research-contract <topic>
```

Then run this command using the contract as the primary instruction artifact.
</command-instruction>

<user-request>
$ARGUMENTS
</user-request>
