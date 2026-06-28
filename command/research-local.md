---
description: 本地资料调研入口，只读取本地文件，不联网搜索，不抓取网页
---

<command-instruction>
Load and follow the `deep-research` skill exactly, but force offline local-file mode and apply the context-first workflow below.

```text
skill(name="deep-research")
```

Also follow these repository rules conceptually:

```text
config/context_policy.md
config/redaction_rules.md
templates/research_contract.md
docs/CONTEXT_FIRST_RESEARCH_WORKFLOW.md
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

## Context-first requirements

- Keep the main session clean: do not paste full PDFs, long extraction logs, or failed parsing traces into the main context.
- For complex internal research, first generate or infer a compact research contract: question, scope, allowed local paths, confidentiality boundary, evidence standard, and output type.
- Preserve source traceability at the local-file level: `claim → local file path → section/page if available → confidence`.
- If local materials are insufficient, report data gaps. Do not silently fill gaps with public knowledge.
- Do not mix multiple customers, projects, internal product lines, or idea threads in the same context unless the contract explicitly asks for cross-project comparison.
- If the context becomes polluted by repeated failed parsing/debug attempts, produce a clean handoff and restart with only the necessary artifacts.

## Parse mode

- `/research-local <path> <topic>` → standard mode
- `/research-local <path> <topic> -quick` → quick mode
- `/research-local <path> <topic> -deep` → deep mode

Pass the request to deep-research as local-file offline research, preserving the user's mode flag.

## Suggested pre-step

For high-stakes, long, or ambiguous local/internal topics, recommend running:

```text
/research-contract <path> <topic>
```

Then run this command using the contract as the primary instruction artifact.
</command-instruction>

<user-request>
$ARGUMENTS
</user-request>
